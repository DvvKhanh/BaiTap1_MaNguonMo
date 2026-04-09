# E. Triển khai (level test) ứng dụng
## 1. Chuyển vào trong thư mục ~/myapp
- Di chuyển vào thư mục: ```cd ~/myapp```

## 2. Gõ lệnh để docker compose chạy: sẽ run tất cả các service khai báo trong file docker-compose.yml
- Chạy lệnh: ```docker-compose up -d```
<img width="578" height="124" alt="image" src="https://github.com/user-attachments/assets/76348a74-1087-4378-97d1-bf35dbea1ad4" />

- Lợi ích: Chỉ cần docker-compose up -d là toàn bộ hệ thống (Web + Node-RED + Tunnel) tự chạy.

## 3. Kiểm tra các container đang chạy trong docker, nếu có cái nào bị restart cần tìm lỗi rồi edit lại docker-compose.yml
- Chạy lệnh: ```docker-compose ps```
<img width="1230" height="176" alt="image" src="https://github.com/user-attachments/assets/78a72258-ee81-464c-a9bd-554e4069bd21" />

## 4. Kiểm tra kiểm thử các service đang chạy độc lập thông qua ip và port của nó: ví dụ mở trình duyệt ip_ubuntu:1880 để check nodered đã chạy chưa
### Kiểm tra Node-RED
- Mở trình duyệt: http://192.168.91.154:1880/
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b99127c2-741c-427f-b2b2-ac67eef4d164" />

### Kiểm tra Flask API (myapi)
- Mở trình duyệt: http://192.168.91.154:9630/
<img width="677" height="285" alt="image" src="https://github.com/user-attachments/assets/cb7be013-80d2-4110-8296-40fbedfc3eb7" />

- Kiểm tra API: http://192.168.91.154:9630/tinh-vat?tien=100
<img width="633" height="408" alt="image" src="https://github.com/user-attachments/assets/d0a0c75f-5d74-4888-b473-3ebcb7f68b2e" />

### Kiểm tra Nginx
- Mở trình duyệt: http://khanh123.id.vn/ hoặc http://192.168.91.154/
<img width="554" height="389" alt="image" src="https://github.com/user-attachments/assets/d40b874b-f1fa-4937-9e45-872344a8af68" />

<img width="454" height="351" alt="image" src="https://github.com/user-attachments/assets/ef6dad68-7533-44cc-a9f8-71ce2df29522" />

## 5. Sử dụng nodered: kéo nodered http_in , http_response, function : để tạo api get đơn giản (dùng cho /api proxy_pass của nginx)
- Truy cập Node-RED: http://192.168.91.154:1880/
- Tạo flow: http in, function, http response
<img width="574" height="422" alt="image" src="https://github.com/user-attachments/assets/0c2f8fb9-5e07-4360-92c7-f7bd4b59c6c8" />

- Test API Node-RED: http://192.168.91.154:1880/api
<img width="702" height="382" alt="image" src="https://github.com/user-attachments/assets/8caf827a-fc8b-4b24-9e66-017468628134" />

## 6. Sửa file ./myweb/index.html : thêm code html+js để sử dụng được api đã khai báo proxy_pass (thực ra là sử dụng nodered http_in hoặc sử dụng service myapi)
- Mở file index.html: ```nano ~/myapp/myweb/index.html```
- Viết code HTML + JS:
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>My Web - Đậu Văn Khánh</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #74ebd5, #9face6);
            text-align: center;
            padding: 50px;
        }

        .container {
            background: white;
            padding: 30px;
            border-radius: 15px;
            width: 400px;
            margin: auto;
            box-shadow: 0 10px 25px rgba(0,0,0,0.2);
        }

        h1 {
            color: #333;
        }

        input {
            padding: 10px;
            width: 80%;
            margin: 10px 0;
            border-radius: 8px;
            border: 1px solid #ccc;
        }

        button {
            padding: 10px 20px;
            border: none;
            border-radius: 8px;
            background: #4CAF50;
            color: white;
            font-size: 16px;
            cursor: pointer;
            transition: 0.3s;
        }

        button:hover {
            background: #45a049;
        }

        .result {
            margin-top: 20px;
            text-align: left;
            background: #f4f4f4;
            padding: 15px;
            border-radius: 10px;
            max-height: 200px;
            overflow: auto;
        }
    </style>
</head>

<body>

<div class="container">

    <h1>🚀 Xin chào, tôi là Đậu Văn Khánh</h1>

    <p>Nhập số tiền để tính VAT:</p>

    <input type="number" id="tien" placeholder="Nhập số tiền...">

    <br>

    <button onclick="callAPI()">💰 Tính tiền</button>

    <div class="result">
        <pre id="result">Kết quả sẽ hiển thị ở đây...</pre>
    </div>

</div>

<script>
function callAPI() {
    let tien = document.getElementById("tien").value;

    if (!tien) {
        document.getElementById("result").innerText = "⚠️ Vui lòng nhập số tiền!";
        return;
    }

    fetch('/api?tien=' + tien)
        .then(response => response.json())
        .then(data => {
            document.getElementById("result").innerText =
                JSON.stringify(data, null, 2);
        })
        .catch(error => {
            document.getElementById("result").innerText =
                "❌ Lỗi: " + error;
        });
}
</script>

</body>
</html>
```

- Kết quả chạy trên trình duyệt: http://khanh123.id.vn/ hoặc http://192.168.91.154/
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/f52bfc06-04f1-4d94-be4f-831554e26dc3" />
