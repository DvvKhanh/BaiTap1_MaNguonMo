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
  + sudo apt update
  + sudo apt install openssh-server -y
  
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
