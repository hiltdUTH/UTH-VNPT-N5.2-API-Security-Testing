# N5.2 – Kiểm thử bảo mật API giữa ứng dụng di động và hệ thống Backend

> **Chương trình:** Học kỳ doanh nghiệp — Trường Đại học Giao thông vận tải TP.HCM (UTH) & VNPT NET  
> **Lĩnh vực:** N5 – An toàn thông tin và bảo mật hệ thống  
> **Thời gian triển khai:** 18 tuần (01/11/2026 – 06/03/2027)  
> **Cơ sở tham chiếu:** OWASP API Security Top 10

---

## 1. Mục tiêu đề tài

Xây dựng quy trình kiểm thử bảo mật API có kiểm soát và có khả năng tái lập giữa ứng dụng di động và hệ thống Backend:

- Xây dựng **Threat Model** và danh mục kiểm thử API theo OWASP API Security Top 10.
- Triển khai môi trường lab cô lập (OWASP crAPI / API mẫu) để thực nghiệm an toàn.
- Kiểm thử có phương pháp các nhóm rủi ro: xác thực, phân quyền đối tượng (BOLA), kiểm tra đầu vào, rate limiting, xử lý lỗi, cấu hình TLS/CORS và ghi log.
- Phân loại rõ phát hiện (**Lỗi xác nhận / Nghi vấn / Khuyến nghị**), đề xuất cách sửa, thực hiện **tái kiểm thử (Re-test)** và hoàn thiện **Checklist Secure API**.

---

## 2. Cấu trúc kho lưu trữ

```text
UTH-VNPT-N5.2-API-Security-Testing/
├── docs/
│   └── weekly-reports/       # Báo cáo tiến độ hàng tuần (Mẫu 2 - PDF)
├── evidence/
│   └── week03/               # Ảnh chụp minh chứng (đã che toàn bộ token/secret)
├── lab/
│   └── docker-compose.yml    # Cấu hình dựng môi trường lab OWASP crAPI
├── postman/
│   └── N5.2_crAPI_Week03.postman_collection.json   # Bộ request & assertion tự động
├── tests/                    # Bộ kiểm thử tự động pytest (triển khai từ Tuần 5)
├── .gitignore                # Chặn file .env, log, khóa bí mật
└── README.md
```

---

## 3. Hướng dẫn tái lập môi trường Lab (3 bước)

**Yêu cầu:** Máy cài sẵn Docker Desktop và Postman.

```bash
# Bước 1: Chuyển vào thư mục lab
cd lab

# Bước 2: Tải image và khởi chạy 10 container OWASP crAPI
docker compose pull
docker compose -f docker-compose.yml --compatibility up -d

# Bước 3: Xác nhận toàn bộ container ở trạng thái Up / Healthy
docker ps
```

**Địa chỉ truy cập sau khi khởi chạy:**
- **Ứng dụng web crAPI:** `http://localhost:8888`
- **Hộp thư giả lập MailHog:** `http://localhost:8025`

**Chạy bộ kiểm thử API Tuần 3:**
1. Mở Postman → chọn **Import** → chọn file `postman/N5.2_crAPI_Week03.postman_collection.json`.
2. Tạo tài khoản giả lập `user_a@example.com` (mật khẩu `LabPass@123`) trên crAPI và kích hoạt xe qua mã VIN/Pincode tại MailHog.
3. Chạy lần lượt 6 request từ `01_Signup_User_B` đến `06_Get_Workshop_Products`.

---

## 4. Quy tắc an toàn & phạm vi kiểm thử

- **Chỉ kiểm thử trên môi trường lab do nhóm sở hữu** (`localhost`) hoặc hệ thống staging có văn bản cấp phép rõ ràng.
- **Tuyệt đối không** quét hoặc kiểm thử hệ thống công khai hay hệ thống sản xuất của VNPT.
- **Không sử dụng dữ liệu thật**; 100% tài khoản và thông tin thử nghiệm là dữ liệu giả lập.
- **Không lưu trữ bí mật trên Git:** toàn bộ JWT token, mật khẩu và thông tin nhạy cảm đều được xóa khỏi file Export và che mờ trong ảnh minh chứng.

---

## 5. Tiến độ thực hiện (Lịch chính thức UTH – VNPT NET)

| Mốc chính | Thời gian | Trạng thái |
|---|---|---|
| Tuần 1–2: Hình thành nhóm & định hướng đề tài | 01/11 – 14/11/2026 | Hoàn thành |
| Tuần 3: Dựng Lab Docker/crAPI & chạy bộ Postman cơ bản | 15/11 – 21/11/2026 | Hoàn thành |
| Tuần 4: Khảo sát endpoint, chốt phạm vi & đo Baseline | 22/11 – 28/11/2026 | Kế hoạch tiếp theo |
| Tuần 5–6: Threat Model & Ma trận Test Case | 29/11 – 12/12/2026 | Chưa bắt đầu |
| Tuần 7–8: Prototype (Postman + pytest + OWASP ZAP) | 13/12 – 26/12/2026 | Chưa bắt đầu |
| Tuần 9: Đánh giá giữa kỳ | 27/12/2026 – 02/01/2027 | Chưa bắt đầu |
| Tuần 10–18: Khắc phục, Tái kiểm thử, Checklist & Bàn giao | 03/01 – 06/03/2027 | Chưa bắt đầu |
