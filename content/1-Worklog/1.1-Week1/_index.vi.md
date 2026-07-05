---
title: "Tuần 1 - Onboarding thực tập và phân tích phạm vi AI1"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 1** — 17/04/2026 – 23/04/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần này đánh dấu sự khởi đầu của kỳ thực tập FCJ Cloud Intern. Tôi tập trung vào việc đọc hiểu đề cương dự án v3.0 (Dataset-Centric / Zeek-First / Decision-Level Fusion) của nhóm và xác định rõ trách nhiệm cụ thể của mình ở vai trò AI1 (Network Anomaly Detection).

### Mục tiêu trong tuần
* Hoàn tất onboarding và làm quen môi trường tài khoản AWS (IAM, Budgets).
* Nghiên cứu kiến trúc Hybrid IDS (Zeek, Suricata, các mô hình AI, Fusion Layer).
* Xác định chính xác phạm vi của mô hình AI1: phát hiện bất thường tầng network sử dụng telemetry từ Zeek `conn.log`.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---:|---|---|---|---|---|
| Ngày 1 | 17/04/2026 | 3 giờ | Hoàn tất thủ tục onboarding và đọc đề cương dự án của nhóm. | Hiểu được mục tiêu tổng thể của nền tảng Hybrid Cloud Security. | Cần làm rõ ranh giới nhiệm vụ giữa các mô hình AI. | Họp nhóm để chia nhỏ phạm vi AI. | Xác định hợp đồng input/output cụ thể của AI1. |
| Ngày 2 | 18/04/2026 | 4 giờ | Xác định scope của AI1 dựa trên chiến lược Zeek-first dataset. | Chốt việc AI1 chỉ tập trung phát hiện bất thường mạng, không phân loại tấn công cụ thể. | Không có vấn đề lớn. | Sử dụng Zeek `conn.log` làm nguồn telemetry chính cho AI1. | Tìm hiểu các dịch vụ AWS hỗ trợ pipeline. |
| Ngày 3 | 20/04/2026 | 5 giờ | Tìm hiểu luồng dữ liệu kiến trúc liên quan đến SQS, AI Engine và Fusion Layer. | Nắm được output của AI1 sẽ đóng vai trò là một bằng chứng (evidence) cho lớp tổng hợp quyết định. | Vai trò của Suricata ban đầu có vẻ chồng chéo với AI1. | Làm rõ rằng Suricata cung cấp rule-based evidence, còn AI1 cung cấp anomaly score dựa trên ML. | Nghiên cứu schema của Zeek `conn.log`. |
| Ngày 4 | 21/04/2026 | 4 giờ | Ôn lại kiến thức AWS IAM và cấu hình AWS Budgets. | Đảm bảo có thể thực hành lab AWS an toàn mà không vượt ngân sách. | Không có vấn đề lớn. | Bắt tay vào thiết lập môi trường thực hành bảo mật. | Chuẩn bị thiết lập Zeek lab. |

### Chi tiết thực hiện
Tôi đã nghiên cứu kỹ kiến trúc **decision-level fusion** của dự án. Tôi nhận thấy mô hình **AI1** của mình sẽ tiêu thụ các đặc trưng mức luồng (flow-level features) được trích xuất từ `conn.log` / `conn_dataset` của Zeek. Output của AI1 sẽ không phân loại đích danh loại tấn công (đó là việc của AI2A) mà chỉ xuất ra nhãn `NORMAL` hoặc `ANOMALY` kèm theo `anomaly_score` hoặc `confidence`. Output này sau đó sẽ được đẩy sang Fusion Layer, nơi tổng hợp kết quả từ AI1, AI2A, AI2B và Suricata evidence để tính toán ra Risk Score cuối cùng.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Ban đầu, tôi khá bối rối không biết AI1 có cần phải xử lý trực tiếp HTTP/DNS log hay phân tích thẳng cảnh báo từ Suricata hay không.
* **Cách giải quyết:** Tôi đã phối hợp cùng team để làm rõ rằng AI1 chỉ xử lý duy nhất flow-level telemetry từ `conn.log`. Các log khác như `http.log` sẽ do AI2B phụ trách, và cảnh báo Suricata sẽ đi thẳng vào Fusion Layer mà không qua AI. Việc này giúp tôi khoanh vùng phạm vi cực kỳ rõ ràng.

### Nhận xét cá nhân
Việc bắt đầu kỳ thực tập bằng cách vạch ra ranh giới rõ ràng cho vai trò AI1 là vô cùng cần thiết. Tôi hiểu rằng nhiệm vụ của mình là xây dựng một mô hình phát hiện bất thường không giám sát thật vững chắc dùng Zeek telemetry, đồng thời tin tưởng vào kiến trúc chung (SQS, Fusion Layer) trong việc định tuyến thông điệp và ra quyết định cảnh báo cuối cùng.
