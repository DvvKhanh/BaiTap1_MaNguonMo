# B. Cài đặt Ubuntu + Docker
## 1. Cài đặt hệ điều hành Ubuntu 24.04.4 LTS
### 1.1. Cài đặt Ubuntu trên VMWare
- Giao diện VMWare:
<img width="1916" height="1116" alt="image" src="https://github.com/user-attachments/assets/2a29d15f-0cbc-4748-bfa5-54e60100a9ad" />

- Tải Ubuntu 24.04.4 LTS
<img width="761" height="46" alt="image" src="https://github.com/user-attachments/assets/c426f257-930d-4fb1-afab-97e81a69218a" />

- Trong giao diện VMWare -> chọn Create a New Virtual Machine -> Typical (recommended)

- Tick chọn Installer disc image file (iso) -> Chọn file Ubuntu .iso vừa tải về
<img width="1900" height="1108" alt="image" src="https://github.com/user-attachments/assets/2989f4a0-dba6-4906-b62a-611098ea4d3e" />

- Nhập tên (Virtual machine name) và nơi lưu máy ảo
<img width="748" height="654" alt="image" src="https://github.com/user-attachments/assets/ca06b671-4278-45de-b818-bb5563491181" />

- Chọn dung lượng ổ cứng:
  + Size: 20GB
  + Chọn: Split virtual disk into multiple files
<img width="937" height="772" alt="image" src="https://github.com/user-attachments/assets/00bfa901-08dd-49ff-8076-0a602f015a25" />

- Cấu hình phần cứng:
  + RAM: 4096 MB
  + CPU: 2 cores
-> Nhấn Finish

<img width="804" height="748" alt="image" src="https://github.com/user-attachments/assets/dfc670a3-9d2c-45be-85a9-6d74db4d2ef8" />

- Giao diện Ubuntu trong VMWare -> chọn Try or Install Ubuntu Server -> nhấn Enter
<img width="1919" height="1143" alt="image" src="https://github.com/user-attachments/assets/3c00986d-702d-48cd-b8ed-98975dd22561" />

- Giao diện sau khi cài xong Ubuntu
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/7d1d45e3-0767-4133-b4f9-45fc0e89b470" />

### 1.2. Cấu hình mạng Ubuntu
#### Cấu hình VMWare:
- Trong VMWare -> chọn VM -> Settings -> Network Adapter
- Chọn: NAT (Recommended)

#### Lấy IP Ubuntu
- Trong Ubuntu gõ: ip a
- Tìm dòng: inet 192.168.91.154
<img width="860" height="252" alt="image" src="https://github.com/user-attachments/assets/9b38bc2a-39e9-4190-bb04-b50f70d5b2f9" />

### 1.3. Cài SSH trên Ubuntu
- Trong Ubuntu chạy:
```
sudo apt update
sudo apt install openssh-server -y
```
  
- Kiểm tra SSH chạy chưa: sudo systemctl status ssh
<img width="733" height="315" alt="image" src="https://github.com/user-attachments/assets/d33458e1-d82e-409e-9453-15686e0b73b3" />

### 1.4. SSH từ Windows
- Nhấn Window + R -> Gõ cmd -> enter
<img width="1475" height="702" alt="image" src="https://github.com/user-attachments/assets/c7b8b910-777d-4698-9bd5-644e6f6fadf3" />

- Gõ lệnh SSH: ssh khanh@192.168.91.154
- Lần đầu sẽ hiện: Are you sure you want to continue connecting (yes/no)? -> nhấn Yes
<img width="1474" height="409" alt="image" src="https://github.com/user-attachments/assets/92676018-122e-471e-915a-0049d2c24b7a" />

- Nhập mật khẩu
<img width="1077" height="395" alt="image" src="https://github.com/user-attachments/assets/dd566cea-a879-4b62-81ee-be22f2800049" />

- Kết quả SSH thành công
<img width="1472" height="681" alt="image" src="https://github.com/user-attachments/assets/8b09a844-ef85-4ff4-823e-48c480eda645" />

## 2. Tìm hiểu các lệnh cơ bản của Ubuntu
### 2.1. Liệt kê các file trong thư mục: ls
- Cú pháp: ls
- Chức năng: Hiển thị danh sách file và thư mục trong thư mục hiện tại.
- Ví dụ:
```
ls -l # hiển thị chi tiết (quyền, kích thước, ngày)
ls -a # hiển thị cả file ẩn
```

### 2.2. Tạo thư mục: mkdir nameFolder
- Cú pháp: mkdir ten_thu_muc
- Chức năng: Tạo thư mục mới.

### 2.3. Chuyển thư mục làm việc: cd path
- Cú pháp: cd duong_dan
- Chức năng: Di chuyển đến thư mục khác.
- Ví dụ:
```
cd baitap # vào thư mục baitap
cd .. # quay lại thư mục trước
cd ~ # về thư mục home
```
### 2.4. Copy file: cp file_nguồn path/file_đích
- Cú pháp: cp file_nguon file_dich
- Chức năng: Sao chép file hoặc thư mục.
- Ví dụ:
```
cp a.txt b.txt # copy file
cp a.txt baitap/ # copy vào thư mục
cp -r folder1 folder2 # copy thư mục
```

### 2.5. Thay đổi quyền thao tác file: sudo chmod xxx filename
- Cú pháp: sudo chmod xxx tenfile
- Chức năng: Thay đổi quyền truy cập file (đọc, ghi, thực thi).
- Ý nghĩa số:

| Số | Quyền |
|----|------|
| 7  | rwx  |
| 6  | rw-  |
| 5  | r-x  |
| 4  | r--  |

- Ví dụ:
```
sudo chmod 777 test.sh # full quyền
sudo chmod 755 file.sh # owner full, others đọc + chạy
```
  
### 2.6. Edit file: sudo nano tenfile
- Cú pháp: sudo nano tenfile
- Chức năng: Mở file để chỉnh sửa trong terminal.
- Ví dụ: sudo nano test.txt
- Phím tắt quan trọng:
  + CTRL+O : lưu nội dung sau khi edit.
  + CTRL+X : thoát edit file

### 2.7. Xem ip của máy ubuntu: ip -4 addr
- Cú pháp: ip -4 addr
- Chức năng: Hiển thị địa chỉ IPv4 của máy Ubuntu.
- Ví dụ kết quả: inet 192.168.91.154/24
-> IP là: 192.168.91.154 (dùng để SSH)

## 3. Cài đặt docker cho Ubuntu
### 3.1. Cập nhật hệ thống
- Mở Ubuntu trong VMWare
- Gõ lệnh:
```
sudo apt update
sudo apt upgrade -y
```
### 3.2. Cài Docker
- Gõ lệnh: sudo apt install docker.io -y
 
### 3.3. Bật Docker và kiểm tra trạng thái
- Gõ lệnh:
```
sudo systemctl enable docker
sudo systemctl start docker
```
- Kiểm tra trạng thái: sudo systemctl status docker
<img width="1367" height="445" alt="image" src="https://github.com/user-attachments/assets/3818d80e-85e3-4227-9bfb-573cb957b48c" />

## 4. Kiểm tra phiên bản docker vừa cài đặt, kiểm tra phiên bản của docker compose
### 4.1. Kiểm tra Docker
- Gõ lệnh: docker --version
<img width="457" height="60" alt="image" src="https://github.com/user-attachments/assets/ca07ca09-7199-45d3-9b68-185f44cc973f" />

### 4.2. Test Docker hoạt động
- Gõ lệnh: sudo docker run hello-world
- Nếu thấy dòng: Hello from Docker! -> Thành công
<img width="698" height="461" alt="image" src="https://github.com/user-attachments/assets/3acb4be4-9545-4708-9e4d-c377417e6381" />

### 4.3. Cài Docker Compose
- Gõ lệnh: sudo apt install docker-compose -y
- Kiểm tra Docker Compose: docker-compose --version
<img width="395" height="61" alt="image" src="https://github.com/user-attachments/assets/7d81a4c5-f69a-451a-bc0c-6d007e25f288" />

## 5. Cấu hình để docker chạy mà không cần tiền tố sudo
- Thêm user vào group docker: sudo usermod -aG docker $USER
- Áp dụng ngay với: newgrp docker
- Test lại: docker run hello-world
<img width="696" height="446" alt="image" src="https://github.com/user-attachments/assets/253455ca-d50e-44e4-aae7-0c299439496c" />

## 6. Tìm hiểu tập lệnh của docker và docker compose
### 6.1. Các tập lệnh của docker
- Xem container đang chạy: docker ps
- Xem tất cả container: docker ps -a
- Xem image: docker images
- docker images: docker pull nginx
- Chạy container: docker run -d -p 80:80 nginx
  + Ý nghĩa: Ý nghĩa: d → chạy nền; p 80:80 → map port
- Dừng container: docker stop <container_id>
- Xóa container: docker rm <container_id>
- Xóa image: docker rmi <image_id>

### 6.2. Các tập lệnh của Docker Compose
- Tạo file: nano docker-compose.yml
- Ví dụ file:
```
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
```
- Chạy: docker-compose up -d
- Dừng: docker-compose down

## 7. Đảm bảo tường lửa trên Ubuntu đã cho phép các cổng 80, 1880, 9630 (Lệnh: sudo ufw allow ...)
- Bật firewall: sudo ufw enable
- Mở port:
```
sudo ufw allow 80
sudo ufw allow 1880
sudo ufw allow 9630
```
<img width="442" height="220" alt="image" src="https://github.com/user-attachments/assets/641d110e-1e44-4b86-b330-02218593a803" />

- Kiểm tra: sudo ufw status
<img width="494" height="271" alt="image" src="https://github.com/user-attachments/assets/2281ff65-3db0-4be1-9a49-4cc03f84dd53" />
