# A. Đăng ký tên miền xịn cho cá nhân
# 1. Đăng ký tên miền domain
## Bước 1: Truy cập: https://tenten.vn/vi/ten-mien/id-vn
## Bước 2: Nhập tên domain
## Bước 3: Xác thực CCCD và thông tin cá nhân
## Bước 4: Hoàn thành đăng ký

## - Đăng ký tên domain thành công:
<img width="1919" height="1011" alt="image" src="https://github.com/user-attachments/assets/d06510d6-6ea3-44cd-9561-2be545d048eb" />

# 2. Đăng ký tài khoản cloudflare
- Truy cập: https://dash.cloudflare.com/
- Đăng ký tài khoản
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/ede3d74f-9018-442b-8912-a34048cf450a" />

## 3. Thêm domain đã đăng ký vào trong cloudflare
- Trong giao diện Cloudflare, chọn Add -> chọn Connect a domain:
<img width="1899" height="985" alt="image" src="https://github.com/user-attachments/assets/0e8892e5-33d4-43f8-8fa4-e15f006633d0" />

- Nhập domain -> nhấn Continue:
<img width="1869" height="998" alt="image" src="https://github.com/user-attachments/assets/5fed0431-c5e3-4720-a62f-a412c454a26a" />

- Trong Free -> chọn Select Plan:
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/3e8bf65d-c03d-441b-ae16-caacf46a626b" />

- Cloudflare sẽ cấp 2 dòng namespace:
<img width="603" height="141" alt="image" src="https://github.com/user-attachments/assets/49a2b508-389c-4acb-93cf-af89d7dc8c9e" />

## 4. Nhập 2 dòng namespace của cloudflare vào trong trang quản lý DNS record của tên miền đăng ký
- Truy cập trang quản lý domain: https://id.tenten.vn/ và đăng nhập tài khoản + mật khẩu
<img width="1919" height="1040" alt="image" src="https://github.com/user-attachments/assets/503b2fa9-e304-4f59-9e6b-04db9f185435" />

- Trong quản lý dịch vụ -> chọn tên miền:
<img width="1919" height="1042" alt="image" src="https://github.com/user-attachments/assets/cbf913c7-b694-4e4d-94e2-ce5c9e4b0aee" />

- Trong thao tác -> chọn Cài đặt NS:
<img width="1492" height="956" alt="image" src="https://github.com/user-attachments/assets/86b2cce6-caf3-4f7d-a35b-abf93807c6cb" />

- Xóa NS cũ và nhập 2 dòng namespace của cloudflare -> Nhấn Cập nhật:
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/a7e30d21-a19c-49a1-a04a-e71c61f24411" />

- Cập nhật thành công:
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/a365e380-0e1b-4640-a208-4696551bbd29" />


