---
title: "Tuần 6 - Đánh giá AI1 và đóng băng artifact"
date: 2026-05-22
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 6** — 22/05/2026 – 28/05/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần này là một cột mốc quan trọng. Tôi đã đánh giá mô hình AI1 bằng các metric tiêu chuẩn, đóng băng (freeze) ngưỡng quyết định tối ưu, và hoàn thiện các artifact kỹ thuật cần thiết để đưa vào triển khai.

### Mục tiêu trong tuần
* Đánh giá AI1 bằng các chỉ số như False Positive Rate và Detection Rate.
* Chốt và đóng băng threshold cho đầu ra `NORMAL`/`ANOMALY`.
* Chuẩn hóa các artifact của AI1 (`model_card.md`, `feature_manifest.json`, `thresholds_frozen.json`).
* Xuất model và preprocessor để dùng cho môi trường inference.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---:|---|---|---|---|---|
| Ngày 1 | 22/05/2026 | 5 giờ | Đánh giá mô hình AI1 trên tập attack data để tính False Positive Rate và Detection Rate. | Thấy rõ sự đánh đổi (trade-off) giữa việc bắt được tấn công và việc gây nhiễu cảnh báo. | Cần chọn một điểm hoạt động (operating point) dứt khoát. | Chọn threshold ưu tiên phạt nặng false positive, vì Fusion Layer cần evidence sạch. | Đóng băng decision threshold. |
| Ngày 2 | 23/05/2026 | 4 giờ | Tham gia AWS Vietnam Community Day và ghi chú các session về AI context, CloudFront, Amazon Quick, LLM non-determinism và multi-agent systems. | Mở rộng góc nhìn về các use case AI và AWS ngoài phần mô hình AI1. | Cần tránh trộn nội dung event với chi tiết triển khai chính của AI1. | Đưa nội dung chi tiết vào Event Participated, trong Worklog chỉ ghi nhận tác động học tập. | Xem lại bài học nào có thể hỗ trợ phần reflection về kiến trúc AI/security. |
| Ngày 3 | 25/05/2026 | 4 giờ | Đóng băng threshold ở mức `0.398066` và ghi nhận decision rule. | Chốt rule: `if confidence >= 0.398066 => ANOMALY`. | Đảm bảo các app phía sau dùng đúng threshold này. | Lưu threshold vào file `thresholds_frozen.json` để load động. | Sinh feature manifest. |
| Ngày 4 | 26/05/2026 | 4 giờ | Tạo file `feature_manifest.json` liệt kê chính xác thứ tự 30 feature. | Đảm bảo tính nhất quán của schema giữa lúc train và inference. | Code inference có thể crash nếu thiếu feature. | Tài liệu hóa rõ ràng policy `fail_if_missing` trong model card. | Dump các file joblib. |
| Ngày 5 | 27/05/2026 | 5 giờ | Export `model.joblib`, `preprocessor.joblib`, và tạo `smoke_samples.jsonl`. | Hoàn thiện gói artifact `AI1_ISOLATION_FOREST_V1`. | Có cân nhắc hướng đi ONNX nhưng quyết định giữ `joblib` cho hiện tại vì tính tương thích runtime. | Chuẩn bị test tích hợp với các file joblib artifact này. | Chuẩn bị tích hợp SQS. |

### Chi tiết thực hiện
Tôi đã đánh giá mô hình `AI1_ISOLATION_FOREST_V1`, tập trung mạnh vào việc giảm thiểu False Positive Rate. Tôi phân tích phân phối điểm số và chính thức đóng băng ngưỡng `selected_threshold = 0.398066`. Luật quyết định (decision rule) được định nghĩa chặt chẽ: `confidence >= 0.398066` sẽ xuất ra `ANOMALY`, thấp hơn là `NORMAL`, tuân thủ nguyên tắc `higher_is_more_anomalous`. Tôi hoàn thiện gói triển khai bằng cách tạo `feature_manifest.json` (chốt cứng 30 đặc trưng), `thresholds_frozen.json`, và export `model.joblib`, `preprocessor.joblib`. Tôi cũng soạn `model_card.md` ghi rõ mục tiêu, scope (`ZEEK_CONN_FLOW_ANOMALY_FEATURES`) và chính sách xử lý dữ liệu `fail_if_missing`. Cuối cùng, tôi tạo file `smoke_samples.jsonl` để các kỹ sư backend dễ dàng test tích hợp.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Việc đảm bảo môi trường inference thực thi chính xác các bước tiền xử lý và thứ tự cột y hệt như môi trường huấn luyện luôn là bài toán MLOps nan giải.
* **Cách giải quyết:** Bằng cách đóng băng `feature_manifest.json` và xuất file `preprocessor.joblib` đi kèm model, tôi đã tách biệt logic này khỏi notebook huấn luyện. Script inference sẽ buộc phải đọc manifest để ép thứ tự cột và áp dụng preprocessor, gần như loại bỏ hoàn toàn rủi ro lệch schema (train-serving mismatch).

### Nhận xét cá nhân
Việc tham dự AWS Vietnam Community Day giúp tôi mở rộng tầm nhìn về kiến trúc AI trên cloud, trong khi các công việc hàng ngày lại rèn luyện sự tỉ mỉ đến từng chi tiết. Đóng băng artifact và viết model card mang lại cảm giác chuyển giao từ khâu "thử nghiệm" sang "kỹ thuật thực thụ". AI1 không còn chỉ là một script chạy trên máy tính cá nhân nữa; nó đã trở thành một thành phần microservice được quản lý phiên bản và có hợp đồng giao tiếp (contract) rõ ràng, sẵn sàng cho việc tích hợp vào pipeline chung.
