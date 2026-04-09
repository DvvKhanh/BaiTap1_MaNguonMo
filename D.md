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

