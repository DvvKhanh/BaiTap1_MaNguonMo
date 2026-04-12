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
- Truy cập link: khanh123.id.vn
