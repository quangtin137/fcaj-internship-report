---
title: "Tuần 7 - Tích hợp pipeline SQS đến AI1 inference"
date: 2026-05-29
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 7** — 29/05/2026 – 04/06/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần này, tôi làm cầu nối giữa công đoạn data engineering và ML operations. Tôi tiến hành tích hợp các artifact AI1 đã đóng băng vào pipeline inference, tiếp nhận và xử lý dòng thông điệp network-flow từ hàng đợi Amazon SQS.

### Mục tiêu trong tuần
* Xây dựng adapter suy luận (inference adapter) để đọc thông điệp từ SQS qua cơ chế batch polling.
* Hiện thực hóa `ai1_inference_common.py` để xử lý logic runtime bằng các artifact đã freeze.
* Đảm bảo adapter định dạng đúng chuẩn output gồm `label`, `confidence`, và `selected_threshold`.
* Chuẩn bị định dạng output của AI1 để đẩy tiếp sang lớp Fusion Layer phía sau.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---:|---|---|---|---|---|
| Ngày 1 | 29/05/2026 | 5 giờ | Rà soát cấu trúc Feature Router và setup SQS batch polling của nhóm. | Hiểu cách AI1 sẽ nhận log Zeek đã parse từ queue. | Schema của message từ SQS ban đầu chưa khớp với định dạng lúc train. | Phối hợp với team đảm bảo message SQS tuân thủ đúng schema flow-level. | Code phần load artifact. |
| Ngày 2 | 30/05/2026 | 5 giờ | Viết `ai1_inference_common.py` để load `model.joblib`, `preprocessor.joblib` và manifest. | Đưa thành công các artifact vào bộ nhớ runtime của quá trình suy luận. | Cần thực thi policy `fail_if_missing` trước khi chạy suy luận. | Viết block kiểm tra xác thực đủ 30 feature dựa theo `feature_manifest.json`. | Viết hàm suy luận cốt lõi. |
| Ngày 3 | 01/06/2026 | 4 giờ | Hoàn thành hàm inference để transform dữ liệu và tính anomaly score. | Mô hình tính toán thành công điểm số thô trên luồng dữ liệu. | Cần áp dụng decision rule đã đóng băng. | Đọc `thresholds_frozen.json` và ánh xạ điểm số ra nhãn `NORMAL` / `ANOMALY`. | Định dạng payload output. |
| Ngày 4 | 02/06/2026 | 5 giờ | Cấu trúc lại output để đảm bảo chứa đủ `label`, `confidence`, và `selected_threshold`. | Chuẩn bị xong hợp đồng output chính xác như Fusion Layer yêu cầu. | Đã test local, nhưng cần xác thực tích hợp trực tiếp với SQS. | Gắn trực tiếp helper suy luận vào script SQS polling. | Test tích hợp. |
| Ngày 5 | 03/06/2026 | 4 giờ | Chạy test tích hợp đẩy `smoke_samples.jsonl` qua toàn bộ pipeline suy luận. | Pipeline nhận thông điệp và trả về nhãn chính xác. | Không có vấn đề lớn. | Tuần tới sẽ đóng gói toàn bộ setup này vào container. | Đọc yêu cầu về containerization. |

### Chi tiết thực hiện
Tôi tập trung xây dựng `ai1_inference_common.py`, chứa logic runtime cốt lõi của AI1. Script này khởi đầu bằng việc load `model.joblib`, `preprocessor.joblib`, `feature_manifest.json`, và `thresholds_frozen.json`. Tôi lập trình cơ chế batch polling để đọc log Zeek từ SQS. Quan trọng nhất, tôi kiểm tra adapter AI1 phải nhận đủ 30 feature theo đúng thứ tự quy định trong manifest. Tuân thủ nghiêm ngặt chính sách `fail_if_missing`, đoạn script sẽ validate payload trước khi gọi preprocessor và model. Sau khi tính ra điểm số thô, nó áp dụng rule `higher_is_more_anomalous` so với `selected_threshold` (0.398066) để xuất ra object cuối cùng chứa `label`, `confidence` và giá trị của ngưỡng.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Xảy ra hiện tượng lệch schema (schema mismatch) trong những lần test tích hợp đầu tiên. Thông điệp từ SQS bị thiếu mất các đặc trưng phái sinh như SSH rolling-windows do Feature Router ở đầu nguồn tính toán chưa đúng trong môi trường realtime streaming.
* **Cách giải quyết:** Thay vì để mô hình dự đoán nhắm mắt trên dữ liệu hỏng, chính sách `fail_if_missing` đã chặn đứng và ném ra lỗi rõ ràng ngay lập tức. Tôi đã báo lại với team để fix lỗi tại Feature Router, đảm bảo có mặt đủ 30 feature mới được đẩy vào AI1 inference adapter.

### Nhận xét cá nhân
Trải nghiệm code pipeline inference mang lại cảm giác cực kỳ thỏa mãn. Nhìn script `ai1_inference_common.py` load mượt mà các artifact, chủ động từ chối payload hỏng, và tự tin xuất ra quyết định `NORMAL`/`ANOMALY` đã chứng minh giá trị của việc đóng băng artifact và lập hợp đồng input/output chặt chẽ. Nó cho thấy một quy trình MLOps mạnh mẽ không chỉ cần khoa học dữ liệu mà cần cả kỹ năng công nghệ phần mềm thực thụ.
