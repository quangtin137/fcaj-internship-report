---
title: "Tuần 8 - Container hóa AI Engine và tích hợp output Fusion"
date: 2026-06-05
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 8** — 05/06/2026 – 11/06/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần này, tôi bước chân vào mảng deployment engineering. Tôi hỗ trợ đóng gói (containerize) AI1 runtime bằng Docker và tìm hiểu cách hệ thống triển khai lên Amazon ECS. Thêm vào đó, tôi đã phối hợp đưa output của AI1 tích hợp vào lớp tổng hợp quyết định (decision-level Fusion Layer) trung tâm của dự án.

### Mục tiêu trong tuần
* Đóng gói các artifact AI1 và script suy luận vào một Docker container.
* Nắm bắt kiến trúc triển khai thông qua dịch vụ Amazon ECS.
* Đẩy output của AI1 sang Fusion Layer để tổng hợp quyết định.
* Đảm bảo AI1 thực hiện đúng vai trò là một "bằng chứng" (evidence) đơn lẻ trong toàn bộ nền tảng Hybrid IDS.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---:|---|---|---|---|---|
| Ngày 1 | 05/06/2026 | 5 giờ | Viết Dockerfile đóng gói Python runtime, `model.joblib`, `preprocessor.joblib` và các script. | AI1 runtime được container hóa thành công. | Cần đảm bảo tương thích artifact trong môi trường Linux container. | Chuẩn bị sẵn các bước check tương thích artifact lúc container startup. | Test container ở local. |
| Ngày 2 | 06/06/2026 | 4 giờ | Chạy Docker container dưới local, nạp `smoke_samples.jsonl` vào chạy thử. | Container xử lý thông điệp hoàn hảo. | Không có vấn đề lớn. | Tiếp tục tìm hiểu cách chạy container này trên ECS. | Đọc tài liệu Amazon ECS. |
| Ngày 3 | 08/06/2026 | 4 giờ | Khám phá Amazon ECS (Elastic Container Service) để hiểu về task definition và scaling. | Nắm được cách team quản lý vòng đời container trên AWS. | Container xác thực với SQS trên ECS bằng cách nào? | Team dùng ECS Task Roles; tôi xác nhận container của mình không hardcode credentials. | Tích hợp vào Fusion Layer. |
| Ngày 4 | 09/06/2026 | 5 giờ | Phối hợp truyền output `label` và `confidence` từ AI1 sang Fusion Layer. | AI1 chính thức đóng góp tham số vào Risk Score cuối cùng. | Fusion Layer yêu cầu cấu trúc JSON đồng nhất từ AI1, AI2A và AI2B. | Kiểm tra lại hợp đồng output AI1 đảm bảo tuân thủ nghiêm ngặt chuẩn JSON. | Quan sát kết quả trên Dashboard. |
| Ngày 5 | 10/06/2026 | 4 giờ | Quan sát Dashboard hiển thị cảnh báo cuối cùng tổng hợp từ nhiều nguồn. | Xác nhận bất thường do AI1 phát hiện hoạt động tốt khi đối chiếu chéo với Suricata log. | Không có vấn đề lớn. | AI1 đã hòa nhập thành công vào nền tảng. | Chuyển sang rà soát dataset mismatch. |

### Chi tiết thực hiện
Tôi đã docker hóa AI Engine cho AI1, đảm bảo tất cả thư viện phụ thuộc và các file `.joblib` được nén gọn gàng. Tôi chuẩn bị các bài test tương thích artifact lúc runtime để chắn chắn `ai1_inference_common.py` hoạt động tốt trên hệ điều hành Linux của container. Tôi cũng dành thời gian nghiên cứu cách team đưa các image này lên Amazon ECS, đặc biệt chú ý đến tính bảo mật của ECS Task Roles trong việc cấp quyền IAM. Điểm tích hợp then chốt là đẩy output của AI1 vào **decision-level fusion**. Tôi kiểm tra kỹ càng xem score direction (`higher_is_more_anomalous`) đã được tài liệu hóa rõ chưa, để Fusion Layer có thể dịch chính xác giá trị `confidence` của AI1. Trong hệ thống này, AI1 đưa ra bằng chứng về bất thường mạng, AI2A phân loại tấn công, AI2B phụ trách tầng application, và Suricata cung cấp cảnh báo dựa trên chữ ký (signature-based).

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Đôi lúc khá khó chấp nhận việc AI1 báo nhãn `ANOMALY` nhưng Dashboard lại không hề chớp cảnh báo, khiến tôi thấy hơi hụt hẫng khi test.
* **Cách giải quyết:** Tôi đã xem lại logic của decision-level fusion cùng team. Tôi hiểu ra rằng AI1 chỉ là một mảnh ghép. Nếu AI1 báo có bất thường nhưng cả AI2A và Suricata đều báo an toàn, Fusion Layer có thể hạ Risk Score xuống để chặn một ca false positive. Điều này đã dạy tôi sức mạnh thực sự của một nền tảng Hybrid Security so với việc phụ thuộc vào một mô hình đơn lẻ.

### Nhận xét cá nhân
Việc đóng gói ứng dụng bằng Docker là cây cầu nối liền đoạn code local của tôi với hạ tầng AWS cloud. Quá trình tích hợp với Fusion Layer mang lại cho tôi sự khiêm tốn; nó chỉ ra rằng trong hệ thống an ninh doanh nghiệp, không có một mô hình ML nào là viên đạn bạc (silver bullet). Vai trò của AI1 là làm một cảm biến (sensor) nhanh, chính xác và đáng tin cậy để nạp dữ liệu vào một ma trận ra quyết định thông minh lớn hơn.
