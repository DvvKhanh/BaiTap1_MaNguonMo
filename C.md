# C. Cấu hình docker compose
## 1. Tạo thư mục: ~/myapp
- Trong Ubuntu, gõ lệnh: ```mkdir ~/myapp```
<img width="373" height="73" alt="image" src="https://github.com/user-attachments/assets/b0d75986-b3bb-40fa-87c8-89219fcb8ae2" />

- Thư mục ~/myapp đã tạo thành công: ```ls ~```
<img width="274" height="83" alt="image" src="https://github.com/user-attachments/assets/8cf88b3b-24d6-4e8d-ab13-2804d76a578c" />

## 2. Chuyển vào trong thư mục ~/myapp
- Trong Ubuntu, gõ lệnh: ```cd ~/myapp```
- Kiểm tra: ```pwd```
<img width="277" height="85" alt="image" src="https://github.com/user-attachments/assets/c7086db8-c561-421f-b4ae-40c3ef3e409c" />

## 3. Tạo thư mục: ./myweb
- Gõ lệnh: ```mkdir myweb nginx nodered```
- Cấu trúc thư mục:
```
myapp/
├── myweb/
├── nginx/
└── nodered/
```

## 4. Tạo file ./myweb/index.html (với nội dung là thông tin cá nhân của em)
- Gõ lệnh: ```nano myweb/index.html```
- Nội dung là thông tin các nhân:
```
<!DOCTYPE html>
<html>
<head>
    <title>My Personal Info</title>
    <meta charset="UTF-8">
</head>
<body>
    <h1>Thông tin cá nhân</h1>
    <p>Họ và tên: Đậu Văn Khánh</p>
    <p>MSSV: K225480106099</p>
    <p>Lớp: K58KTP</p>
</body>
</html>
```

- Lưu:
  + ctrl+O -> Enter
  + Ctrl + X
 
## 5. Tạo file docker-compose.yml
- Gõ lệnh: ```nano docker-compose.yml```
- Nội dung file: ```docker-compose.yml```
```
version: '3.8'

services:
  mynodered:
    image: nodered/node-red:latest
    container_name: mynodered
    ports:
      - "1880:1880"
    volumes:
      - ./nodered:/data
    restart: unless-stopped

  mynginx:
    image: nginx:latest
    container_name: mynginx
    ports:
      - "80:80"
    volumes:
      - ./myweb:/myweb:ro
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - mynodered
    restart: always
```

- Lưu lại.

## 6. Edit file ./nginx/nginx.conf
- Gõ lệnh: ```nano nginx/nginx.conf```
- Nội dung file: ```./nginx/nginx.conf```
```
server {
        listen 80;
        server_name khanh123.id.vn;

        location / {
            root /myweb;
            index index.html;
        }

        location /api {
            proxy_pass http://nodered:1880;
        }
    }
```

## 7. Edit file ./nodered/settings.js để nodered bắt buộc đăng nhập
### Bước 1: Chạy lần đầu: ```docker-compose up -d```
<img width="655" height="635" alt="image" src="https://github.com/user-attachments/assets/e9e9f989-1b6e-4ff8-a38e-6c3b51103c56" />

### Bước 2: Kiểm tra: ```docker compose ps```
<img width="1248" height="148" alt="image" src="https://github.com/user-attachments/assets/058143ec-fab1-453b-acf5-917fd87bd29a" />

### Bước 3: Dừng container: ```docker compose down```
<img width="369" height="117" alt="image" src="https://github.com/user-attachments/assets/0553358b-eedd-44cd-bd71-7d7dc574fc79" />

### Bước 4: Tạo password hash: ```docker exec -it mynodered node-red-admin hash-pw```
<img width="871" height="95" alt="image" src="https://github.com/user-attachments/assets/0a379e87-8b38-4449-b974-1d9cd392d53f" />

- Sau khi gõ lệnh, hệ thống hiện Password:, bạn hãy nhập mật khẩu mới của mình (ví dụ: khanh123) và nhấn Enter.
- Nó sẽ trả về một chuỗi ký tự dài bắt đầu bằng 
```$2b$... hoặc $2a$....```
- Hãy bôi đen và Copy chuỗi ký tự đó.

### Bước 5: Dán mật khẩu mới vào cấu hình
- Mở file settings.js để chỉnh sửa: ```nano ~/myapp/nodered/settings.js```
- Tìm đến đoạn adminAuth và thay thế chuỗi password cũ bằng chuỗi bạn vừa copy:
```
adminAuth: {
    type: "credentials",
    users: [{
        username: "admin",
        password: "$2y$08$ytWvB.r5McY77jstkKSWCOAsV7sK6s80EK83R/vN6NlGzKN21ZZT6",
        permissions: "*"
    }]
},
```

### Bước 6: Khởi động lại để áp dụng
- Để Node-RED đọc lại file cấu hình mới, bạn phải restart container:
```
docker-compose restart mynodered
```

- Truy cập domain: http://khanh123.id.vn/ sẽ thấy thông tin cá nhân (nội dung file index.html)
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d8e09083-2dcd-4663-a516-01219f0e2d40" />

- Nodered cần truy cập đường link: http://192.168.91.154:1880, Nodered sẽ yêu cầu đăng nhập
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/741e9633-7e15-4d9a-bb34-4f857395963b" />

- Giao diện Nodered
<img width="1919" height="1083" alt="image" src="https://github.com/user-attachments/assets/8f0f3c8a-2755-4183-81a5-edd858b67624" />

- Nginx:
  + Tạo flow API: http in, function, http response
<img width="685" height="100" alt="image" src="https://github.com/user-attachments/assets/bc221943-73b9-42da-a57f-5acafed23a4f" />

  + Truy cập trực tiếp trên nodered: http://192.168.91.154:1880/api
<img width="624" height="422" alt="image" src="https://github.com/user-attachments/assets/c29e9064-9045-411a-a3c2-f17d3f2646ba" />

  + Truy cập qua domain: http://khanh123.id.vn/api
<img width="545" height="410" alt="image" src="https://github.com/user-attachments/assets/8a1a483a-de1b-4d40-b033-30665dd7a19d" />
