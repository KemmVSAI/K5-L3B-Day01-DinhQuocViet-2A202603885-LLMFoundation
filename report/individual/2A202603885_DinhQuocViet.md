# Báo cáo cá nhân — Day 01: Lọc tin nhắn rác bằng ML, DL, Embedding và LLM

> Đổi tên file này thành `MSSV_HoTen.md` của bạn, ví dụ `2A20260000_NguyenVanAn.md`, rồi điền từng mục. Xoá các dòng hướng dẫn in nghiêng khi xong.

- **MSSV: 2A202603885**
- **Họ và tên: Đinh Quốc Việt**
- **SEED** (in ở ô đầu notebook):L ab Ngày 1 — Một bài toán, bốn thế hệ AI: Lọc tin nhắn rác (Spam / Ham)

## 1. Bằng chứng

| Tệp | Có? |
| --- | --- |
| [`day01_lab.ipynb`](day01_lab.ipynb) — notebook đã chạy, còn output | không còn |
| [`lab1_report.json`](lab1_report.json) — do ô "Báo cáo cuối" sinh ra | có |
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | có |

## 2. Kết quả chính

*Chép bảng so sánh cuối Phần 5 (F1 test, F1 SUB 40, p95 ms/tin, USD/1k tin) của chính bạn.*
Model	F1 test	F1 SUB 40	p95 (ms/tin)	USD/1k tin
BoW + LogReg	0.845	0.750	0.339	0
BoW + MLP	0.921	0.857	1.917	0
MiniLM + kNN	0.866	0.889	61.669	0
MiniLM + LogReg	0.939	0.857	37.518	0
MiniLM + MLP head	0.944	0.889	38.371	0
LLM nhỏ zero-shot	0.919	0.919	1653.841	0.006
LLM lớn zero-shot	0.974	0.974	1665.246	0.065
LLM nhỏ few-shot	0.947	0.947	1597.848	0.015

## 3. Dự đoán và thực tế

*Chọn 3 dự đoán `predict(...)` lệch nhiều nhất so với kết quả. Mỗi cái: bạn đoán gì, ra gì, vì sao lệch.*


## 4. Phân tích 50 case khó

*Model nào tụt nhiều nhất, ở nhóm case nào? Nêu 2 case cụ thể và cơ chế khiến model sai.*
Model tụt nhiều nhất là BoW + MLP:
- F1 test thường: 0.857.
- F1 trên tập khó: 0.419.
- Mức giảm: 0.438.
Nhóm khó nhất:
- BoW gặp khó với spam giọng tự nhiên (0%) và né từ khóa (29%).
- Embedding gặp khó với nhóm nói về spam: kNN đạt 40%, LogReg và MLP đạt 60%.
- LLM 4a và 4d đạt 100% ở tất cả nhóm trong thử nghiệm.

## 5. Thử thách cá nhân

*Đề theo SEED của bạn, cách làm, kết quả, giải thích.*
Em thay đổi số nơ-ron lớp ẩn của MLP thành 4 và 512.

| Chỉ số | 4 nơ-ron | 512 nơ-ron |
|---|---:|---:|
| Số tham số | 8.009 | 1.025.025 |
| F1 test | 0.9205 | 0.9153 |
| Thời gian train | 28.64 giây | 55.04 giây |

Kết quả cho thấy MLP 4 nơ-ron đạt F1 cao hơn và huấn luyện nhanh hơn. Em nhận ra mô hình lớn hơn không phải lúc nào cũng tốt hơn. Mạng nhỏ phù hợp khi dữ liệu không quá phức tạp và cần tiết kiệm tài nguyên.

## 6. Phương án cho sản phẩm thật

*Với yêu cầu p95 < 50 ms/tin cho 10 triệu SMS/ngày: chọn phương án nào, dẫn số liệu nào, chấp nhận đánh đổi gì.*
Em chọn BoW + MLP.

Lý do:
- F1 test: 0.921.
- F1 SUB 40: 0.857.
- p95: 1.917 ms/tin, đáp ứng yêu cầu dưới 50 ms.
- Chi phí: 0 USD/1.000 tin.
Với 10 triệu SMS/ngày, trung bình cần xử lý khoảng 116 tin/giây. BoW + MLP có tốc độ nhanh, chi phí thấp và có thể triển khai trên máy chủ riêng.

Đánh đổi: Mô hình dễ bỏ sót spam viết tự nhiên hoặc né từ khóa. Vì vậy cần bổ sung dữ liệu thực tế, chuẩn hóa văn bản và giám sát kết quả thường xuyên. Cần benchmark lại khi có tải lớn để xác nhận p95 dưới 50 ms.

## 7. AI log và điều đã học

*Mỗi lần dùng AI: hỏi gì, nhận gì, bạn sửa gì. Sau đó 2–3 câu: điều bạn hiểu ra hôm nay mà trước đó chưa hiểu.*
Em hiểu rằng F1 cao trên tập test thường chưa đủ để chứng minh mô hình hoạt động tốt trong thực tế. Cần kiểm tra thêm các tình huống khó và đánh giá cả tốc độ, chi phí. Em cũng nhận ra mỗi loại mô hình có ưu nhược điểm khác nhau, nên cần chọn mô hình phù hợp với yêu cầu sản phẩm thay vì chỉ nhìn vào độ chính xác.
