# G. Triển khai ứng dụng đến End-user
## 1. Trong Cloudflare: Tạo tunnel (đường hầm), chọn loại triển khai cho docker
- Truy cập link: https://dash.cloudflare.com/
- Trong giao diện Cloudflare -> Chọn Recents -> chọn Zero Trust
- Giao diện Zero Trust -> Chọn Get started

<img width="1920" height="1200" alt="Screenshot 2026-04-12 204622" src="https://github.com/user-attachments/assets/1be43044-d165-4cd3-8bbd-0c6dabb4de2f" />

- Chọn Networks -> Connectors -> Create a tunnel
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/385ea4ac-d230-481e-b5e9-41f6d1a54d37" />

- Chọn Select Cloudflared
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b2596e1d-13e4-4fe0-aebc-826d248220a3" />

- Nhập Tunnel name -> Save tunnel
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c48537c3-1d4f-41ec-a861-52cc612f9380" />

## 2. Convert lệnh docker run ... sang dạng docker compose
- Sau khi nhập Tunnel name -> Chọn loại: Docker -> Cloudflare sẽ hiện token -> Copy
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/1c637bcb-8895-441a-a9d8-e3b85d62dbae" />

## 3. Khai báo kết quả convert vào trong file docker-compose.yml
- Mở file docker-compose.yml: 
  + Thêm service vào file docker-compose.yml
```
mycloudflared:
    image: cloudflare/cloudflared:latest
    container_name: mycloudflared
    restart: unless-stopped
    command: tunnel --no-autoupdate run --token eyJhIjoiZjA4MWIzNDRiZGJkZmZjNjA3YzdjMjA0ODNmODJlZGUiLCJ0IjoiY2U4ZWVhODU>
```
<img width="1481" height="761" alt="image" src="https://github.com/user-attachments/assets/0d656708-8e0e-448b-8f68-84caf8716ca2" />

## 4. Chạy lại docker compose
- Chạy lệnh:
```
docker compose down
docker compose up -d
```

- Kiểm tra
```
docker-compose ps
```
<img width="1285" height="167" alt="image" src="https://github.com/user-attachments/assets/ca57135f-f6cc-40d8-b009-0b24d1b117ca" />

## 5. Public ứng dụng bằng cách thêm 1 router trỏ tới container đang chạy trong docker, dữ liệu sẽ đi qua tunnel, url dạng sub-domain
- Quay lại Cloudflare
- Vào lại Networks -> Connectors
- Trong giao diện Connectors -> Chọn my-tunnel vừa tạo
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d0e7aa09-7e40-4481-82bb-0e0c776a143d" />

- Chọn Add a published application route
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/fb4d39ef-293c-401a-8ce3-cd18faed3356" />

- Nhập thông tin:
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/2f426d44-aec3-43a7-98e6-3ce403c0a410" />

```
| Trường    | Giá trị             |
| --------- | ------------------- |
| Subdomain | Để trống            |
| Domain    | `khanh123.id.vn`    |
| Path      | Để trống            |
| Type      | HTTP                |
| URL       | mynginx:80          |
```

## 6. Kiểm tra url sub-domain đã hoạt động public cho mọi end-user
- Mở web: https://app.khanh123.id.vn/
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/712ff037-ba1f-485a-8bf0-0b0b8bb30fb6" />

- Test API: http://app.khanh123.id.vn/api?tien=100
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/1a637a10-06ab-4ed3-98c0-57cee307a5ce" />

- Truy cập trên điện thoại:
<img width="828" height="1792" alt="image" src="https://github.com/user-attachments/assets/74d5633e-63c4-46ab-8ed8-c8f6cf56cf78" />

# Câu hỏi về bài tập
## 1. Tại sao phải dùng Nginx làm Reverse Proxy mà không trỏ thẳng Tunnel vào Node-RED?
- Nginx giúp điều phối nhiều service trên cùng 1 domain:
  + / → web (index.html)
  + /api → Node-RED hoặc Flask
- Ẩn backend (không lộ port 1880, 9630) → tăng bảo mật.
- Dễ mở rộng: thêm nhiều API/service mà không cần thêm domain.

-> Nếu không dùng Nginx → chỉ truy cập được 1 service duy nhất.

## 2. Sự khác biệt giữa việc Mount file và Mount thư mục trong Docker là gì?
- Mount thư mục:
  + Gắn cả thư mục (./myweb:/myweb).
  + Dùng cho HTML, dữ liệu, source code.
  + Sửa file → cập nhật ngay.
- Mount file:
  + Gắn 1 file (nginx.conf).
  + Dùng cho cấu hình.

-> Tóm lại: thư mục = dữ liệu, file = cấu hình

## 3. Nếu thay đổi file index.html ở máy Ubuntu, nội dung trên web có thay đổi ngay không? Tại sao?
- Có, cập nhật ngay.
- Vì dùng bind mount (./myweb:/myweb).
- Nginx đọc trực tiếp file trên máy Ubuntu.

-> Không cần restart container.

## 4. Docker-compose.yml khai báo các services có phần restart: always hoặc restart: unless-stopped : chúng để làm gì?
- restart: always: Container tự chạy lại khi bị lỗi hoặc reboot máy
- restart: unless-stopped: Tự chạy lại, trừ khi bị stop thủ công

-> Giúp hệ thống chạy ổn định, tránh bị tắt dịch vụ

## 5. Cách khai báo để tất cả các services đều dùng chung 1 network? lợi ích của việc khai báo này là gì? Sửa đổi file docker-compose để tất cả các service đều dùng chung 1 network.
- Khai báo:
```
networks:
  mynetwork:
```
- Gắn vào service:
```
services:
  myapi:
    networks:
      - mynetwork
```
- Lợi ích:
  + Container gọi nhau bằng tên (myapi, mynginx).
  + Không cần dùng IP.
  + Dễ quản lý và ổn định hơn.

## 6. Tìm cách đưa Cloudflare Token vào trong file .env rồi sau đó thêm .env vào file .gitignore trước khi push code lên github. Tại sao nói đây là điều quan trọng về bảo mật mã nguồn?
- Tạo file .env:
```
CLOUDFLARE_TOKEN=abc123
```
- Dùng trong docker-compose:
```
command: tunnel --token ${CLOUDFLARE_TOKEN}
```
- Thêm .env vào .gitignore
- Lý do:
  + Token là thông tin nhạy cảm.
  + Nếu push lên GitHub → có thể bị lộ và bị hack

-> Đây là nguyên tắc bảo mật quan trọng

## 7. Tại sao chúng ta nên thêm hậu tố :ro khi mount file cấu hình Nginx?
- :ro = read-only (chỉ đọc)
- Container không thể sửa file.
- Lợi ích:
  + Tránh lỗi cấu hình.
  + Tăng bảo mật.

-> Bảo vệ file nginx.conf

## 8. Khi dùng Cloudflare Tunnel: có cần thiết phải mở cổng cho các service nữa không?
- Không cần mở port ra Internet.
- Vì tunnel tạo kết nối từ: Ubuntu → Cloudflare
- Ưu điểm:
  + Không cần NAT / port forwarding.
  + An toàn hơn.
