# F. Gỡ lỗi
## 1. Nếu có lỗi xẩy ra trong quá trình triển khai docker compose up -d
### Kiểm tra trạng thái container
- Gõ lệnh: ```docker-compose ps```
<img width="1236" height="171" alt="image" src="https://github.com/user-attachments/assets/54ea06a4-b173-4c82-8f6c-041915603a54" />

- Lệnh: ```docker-compose ps``` giúp xác định nhanh:
  + Up -> chạy bình thường.
  + Restarting -> container bị lỗi và đang khởi động lại liên tục.
  + Exited -> container bị lỗi và đã dừng.

### Xem log container bị lỗi
```
docker logs mynginx
docker logs myapi
docker logs mynodered
```
- Mục đích:
  + Xem nguyên nhân lỗi.
  + Tập trung đọc dòng cuối cùng của log.

### Phân tích lỗi theo từng service
- mynginx Restarting: thường do sai cấu hình trong nginx.conf (sai proxy_pass, sai tên service, lỗi cú pháp).
- myapi Exited / Restarting, thường do:
  + Lỗi code app.py
  + Thiếu thư viện trong requirements.txt
  + Lỗi trong Dockerfile
- mynodered Restarting, thường do:
  + Lỗi settings.js
  + Sai quyền thư mục ./nodered

### Sửa lỗi và build lại
- Gõ lệnh: ```docker compose up -d --build```
- Dùng --build để cập nhật lại code sau khi sửa.

### Kiểm tra lại
- Gõ lệnh: ```docker compose ps```
- Tất cả container phải ở trạng thái: ```Up```

## 2. Thêm healthcheck cho myapi trong file docker-compose.yml
- Mở file: ```nano docker-compose.yml```
- Thêm vào service myapi:
```
healthcheck:
    test: ["CMD", "curl", "-f", "http://localhost:9630"]
    interval: 30s
    timeout: 10s
    retries: 3
    start_period: 10s
```
<img width="1472" height="755" alt="image" src="https://github.com/user-attachments/assets/3364bd38-9fa0-4e56-a61f-52af325d5d4f" />

- Giải thích:
  + test: dùng lệnh curl để kiểm tra API có phản hồi không.
  + interval: mỗi 30 giây kiểm tra 1 lần.
  + timeout: quá 10 giây không phản hồi -> lỗi.
  + retries: thử lại 3 lần trước khi kết luận lỗi.
  + start_period: khoảng thời gian chờ sau khi container vừa khởi động (ví dụ 10 giây), trong thời gian này nếu check lỗi cũng không bị tính -> tránh báo lỗi sai khi service chưa kịp chạy.

- Để cơ chế healthcheck hoạt động chính xác, image myapi cần được trang bị công cụ curl. Tuy nhiên, với base image python:3.9-slim, tiện ích này thường không được cài đặt sẵn. Vì vậy, trong trường hợp healthcheck báo lỗi mặc dù ứng dụng vẫn hoạt động bình thường, cần bổ sung bước cài đặt curl trong Dockerfile để đảm bảo quá trình kiểm tra trạng thái dịch vụ diễn ra đúng.
- Gõ lệnh: ```nano ~/myapp/myapi/Dockerfile```
<img width="1479" height="758" alt="image" src="https://github.com/user-attachments/assets/7f96298b-004d-4268-bcd2-5874fb299ed7" />

- Build lại: ```docker compose up -d --build```
## 3. Giới hạn resource cho một service (tránh việc 1 service chiếm quá nhiều ram)
- Mở file: ```nano docker-compose.yml```
- Thêm vào myapi trong docker-compose.yml:
```
deploy:
    resources:
      limits:
        memory: 512M
```
<img width="1473" height="755" alt="image" src="https://github.com/user-attachments/assets/e09af682-2fd7-48b8-bdfa-b9a43ae0cfdf" />

- Giải thích: Giới hạn container chỉ sử dụng tối đa 512MB RAM -> tránh 1 service chiếm hết tài nguyên máy

## 4. Theo dõi tài nguyên
- Gõ lệnh: ```docker stats```
- Kết quả:
<img width="1470" height="217" alt="image" src="https://github.com/user-attachments/assets/ed33b9d9-4b71-41e2-bfd3-7b51dddba0aa" />

- Ý nghĩa:

  Giúp theo dõi:
  + Container nào dùng nhiều RAM
  + Phát hiện service bất thường
