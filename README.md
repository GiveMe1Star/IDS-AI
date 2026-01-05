# 🛡️ IDS-AI: Network Intrusion Detection System

Hệ thống phát hiện xâm nhập mạng sử dụng trí tuệ nhân tạo (AI). Dự án cung cấp REST API để dự đoán và giao diện web để phân tích thủ công.

## 📁 Cấu trúc thư mục

```
IDS-AI/
├── backend/                  # API server
│   ├── app.py               # Flask application chính
│   └── test_api.py          # Script test API
├── frontend/                 # Giao diện người dùng
│   ├── App_UI.html          # Giao diện chính (đầy đủ tính năng)
│   └── frontend.html        # Giao diện đơn giản
├── models/                   # Machine Learning models
│   ├── nids_logistic_regression.pkl    # Model chính (Logistic Regression)
│   ├── xgboost_model.pkl               # Model XGBoost
│   ├── intrusion_detection_model.pkl   # Model thay thế
│   ├── X_test.pkl                      # Test features
│   └── y_test.pkl                      # Test labels
├── data/                     # Dữ liệu và logs
│   ├── snort_logs.csv       # Log từ Snort IDS
│   ├── snort_insights.csv   # Phân tích Snort
│   ├── synthetic_dataset.xlsx
│   └── alert                # Alert logs từ Snort
├── notebooks/                # Jupyter notebooks
│   └── NIDS (1).ipynb       # Notebook phân tích và training
├── scripts/                  # Scripts tiện ích
│   └── snort_log_to_csv.py  # Chuyển đổi Snort log sang CSV
└── README.md
```

## 🚀 Cài đặt

### Yêu cầu
- Python 3.10+
- pip

### Cài đặt dependencies

```bash
pip install flask flask-cors joblib numpy requests
```

## 💻 Sử dụng

### 1. Khởi động API Server

```bash
cd backend
python app.py
```

Server sẽ chạy tại `http://127.0.0.1:5000`

### 2. Mở giao diện web

Mở file `frontend/App_UI.html` trong trình duyệt (đảm bảo server đang chạy).

### 3. Test API

```bash
cd backend
python test_api.py
```

Hoặc dùng curl:

```bash
curl -X POST http://127.0.0.1:5000/predict \
  -H "Content-Type: application/json" \
  -d '{"features": [0, 3, 1, 0, 4578, 9254, 0, 0, 1, 1, 5, 0, 1, 1, 0, 1, 0, 1, 0, 0]}'
```

## 📊 Các đặc trưng (Features)

Hệ thống phân tích 20 đặc trưng mạng:

| # | Tên | Mô tả |
|---|-----|-------|
| 1 | Duration | Thời gian kết nối (giây) |
| 2 | Protocol Type | Loại giao thức (TCP, UDP, ICMP) |
| 3 | Service | Dịch vụ mạng đích |
| 4 | Flag | Trạng thái kết nối |
| 5 | Source Bytes | Bytes gửi từ nguồn |
| 6 | Destination Bytes | Bytes gửi từ đích |
| 7 | Land | Kết nối cùng host/port |
| 8 | Wrong Fragment | Số fragment lỗi |
| 9 | Urgent | Số gói tin urgent |
| 10 | Hot | Số chỉ báo "hot" |
| 11 | Failed Logins | Số lần đăng nhập thất bại |
| 12 | Logged In | Đăng nhập thành công (0/1) |
| 13 | # Compromised | Số điều kiện bị xâm phạm |
| 14 | Root Shell | Có shell root (0/1) |
| 15 | Su Attempted | Thử lệnh su (0/1) |
| 16 | # Root | Số truy cập root |
| 17 | # File Creations | Số file được tạo |
| 18 | # Shells | Số shell prompts |
| 19 | # Access Files | Số thao tác với access control files |
| 20 | # Outbound Commands | Số lệnh outbound trong FTP |

## 🔧 Scripts tiện ích

### Chuyển đổi Snort log

```bash
cd scripts
python snort_log_to_csv.py
```

Chuyển đổi alert log của Snort IDS thành file CSV để phân tích.

## 📝 API Response

```json
{
  "prediction": 1,
  "probability": [[0.15, 0.85]]
}
```

- `prediction`: `0` = Bình thường, `1` = Xâm nhập
- `probability`: Xác suất cho mỗi class

## 🤝 Đóng góp

1. Fork repository
2. Tạo branch mới (`git checkout -b feature/TinhNangMoi`)
3. Commit thay đổi (`git commit -m 'Thêm tính năng mới'`)
4. Push lên branch (`git push origin feature/TinhNangMoi`)
5. Tạo Pull Request

## 📄 License

MIT License
