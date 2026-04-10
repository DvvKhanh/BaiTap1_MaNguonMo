# D. (Bonus - không bắt buộc)
## 1. Tạo thư mục ./myapi
```
cd ~/myapp
mkdir myapi
cd myapi
```
<img width="468" height="101" alt="image" src="https://github.com/user-attachments/assets/6e74ad01-9929-4dda-987c-f0ef04042ab3" />

## 2. Tạo file ./myapi/app.py sử dụng Python + Flask để làm gì đó funny
- Gõ lệnh: ```nano app.py```
- Code file app.py
```
from flask import Flask, request, jsonify
import datetime

app = Flask(__name__)

@app.route('/tinh-vat', methods=['GET'])
def tinh_vat():
    tien_input = request.args.get('tien')

    if tien_input is None:
        return jsonify({
            "trang_thai": "lỗi",
            "thong_bao": "Vui lòng nhập ?tien=..."
        }), 400

    try:
        so_tien = float(tien_input)
        tong = so_tien * 1.1

        return jsonify({
            "trang_thai": "thành công",
            "thong_bao": "💰 Tính tiền thành công!",
            "du_lieu": {
                "tien_goc": so_tien,
                "vat": "10%",
                "tong_cong": tong,
                "thoi_gian": str(datetime.datetime.now()),
                "note": "Tiền nhiều để làm gì 😎"
            }
        })
    except:
        return jsonify({
            "trang_thai": "lỗi",
            "thong_bao": "Tiền phải là số!"
        }), 400


@app.route('/')
def home():
    return "🚀 Flask API đang chạy!"


if __name__ == '__main__':
    app.run(host='0.0.0.0', port=9630)
```

## 3. Tạo file ./myapi/requirements.txt chứa các thư viện mà app.py sử dụng (theo như app.py ví dụ thì requirements.txt chỉ cần có nội dung: flask)
- Gõ lệnh: ```nano requirements.txt```
- Nội dung: ```flask```

## 4. Tạo file ./myapi/Dockerfile để khai báo sử dụng Python 3.9 slim
- Tạo Dockerfile, gõ lệnh: ```nano Dockerfile```
- Nội dung:
```
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 9630

CMD ["python", "app.py"]
```

## 5. Sửa đổi docker-compose để sử dụng myapp
- Gõ lệnh:
```
cd ~/myapp
nano docker-compose.yml
```
- Thêm service myapi:
```
services:
  myapi:
    build:
      context: ./myapi
      dockerfile: Dockerfile
    container_name: myapi
    ports:
      - "9630:9630"
    restart: always
```

- Kiểm tra container myapi: docker compose ps
<img width="1240" height="167" alt="image" src="https://github.com/user-attachments/assets/80405b6a-16c9-4701-bbb5-e3ced07468c8" />

## 6. Sửa đổi nginx/nginx.conf để /api trỏ tới service myapp cổng 9630
- Gõ lệnh: ```nano ~/myapp/nginx/nginx.conf```
- Sửa đoạn /api:
```
location /api {
    proxy_pass http://192.168.91.154:9630/tinh-vat;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}
```

- Build & chạy lại:
```
docker compose down
docker compose up -d --build
```

- Mở trình duyệt: http://192.168.91.154:9630/
<img width="677" height="285" alt="image" src="https://github.com/user-attachments/assets/cb7be013-80d2-4110-8296-40fbedfc3eb7" />

- Kiểm tra trực tiếp Flask: http://192.168.91.154:9630/tinh-vat?tien=100
<img width="633" height="408" alt="image" src="https://github.com/user-attachments/assets/80ea825f-f05b-4cd1-8649-fa8ed7b60fce" />
