---
title: "Tuần 8 - Trích xuất đặc trưng Zeek và tìm hiểu containerization"
date: 2026-06-05
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
{{% notice info %}}
**Worklog Tuần 8** - 05/06/2026 - 11/06/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần 8 tiếp tục hướng Zeek bằng công việc trích xuất feature cụ thể hơn. Tôi cũng tìm hiểu Docker và ECS để hiểu AI Engine có thể được đóng gói về sau như thế nào. Phần model vẫn ở mức thử nghiệm: tạo feature cơ bản và phái sinh từ Zeek, thử train lại Isolation Forest ban đầu và rà soát output format dự kiến với nhóm.

### Mục tiêu trong tuần
* Ôn Docker cơ bản và khái niệm ECS cho thành phần AI dạng container.
* Xây dựng hoặc điều chỉnh feature extraction cho các field conn.log đã chọn.
* Tạo feature phái sinh cơ bản từ byte count, duration và categorical field.
* Chạy thử train lại ở mức ban đầu và rà soát output format dự kiến của AI1.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---|---|---|---|---|---|
| Ngày 1 | 05/06/2026 | 4 giờ | Tìm hiểu Docker image, container và lý do cần đóng gói service. | Hiểu hướng container hóa AI Engine về sau. | Chi tiết containerization có thể làm phân tán khi feature vẫn chưa ổn. | Giữ ghi chú Docker ở mức tổng quan. | Tiếp tục extraction từ Zeek. |
| Ngày 2 | 06/06/2026 | 5 giờ | Bắt đầu trích xuất các field đã chọn từ mẫu Zeek conn.log. | Tạo được bảng feature đầu tiên từ connection record. | Một số dòng thiếu service hoặc byte value. | Áp dụng rule parse đã phác thảo ở Tuần 7. | Thêm feature phái sinh đơn giản. |
| Ngày 3 | 08/06/2026 | 4 giờ | Tạo feature phái sinh như byte ratio và giá trị liên quan duration. | Feature set tốt hơn so với chỉ dùng cột thô. | Feature phái sinh có thể lỗi khi mẫu số bằng 0 hoặc thiếu. | Thêm guard logic cho phép tính không an toàn. | Chạy thử retraining nhỏ. |
| Ngày 4 | 09/06/2026 | 5 giờ | Thử train lại Isolation Forest trên feature dựa trên Zeek. | Có score ban đầu để kiểm tra. | Sample size và độ ổn định feature vẫn còn giới hạn. | Chỉ gọi đây là thử nghiệm ban đầu. | Rà soát output kỳ vọng với nhóm. |
| Ngày 5 | 10/06/2026 | 3 giờ | Rà soát các field output AI1 dự kiến với nhóm. | Thống nhất sơ bộ loại label/score mà component phía sau có thể cần. | Integration contract chưa hoàn toàn chốt. | Chuẩn bị output format draft, không viết là đã tích hợp xong. | Sắp xếp ghi chú thử nghiệm. |

### Chi tiết thực hiện
Docker và ECS được học để hiểu hình dạng triển khai, không phải để khẳng định đã có container hoàn chỉnh. Phần này giúp tôi thấy AI Engine về sau cần file, dependency và runtime assumption nào.

Feature extractor bắt đầu chuyển record conn.log thành input cho model. Các field số cơ bản được kết hợp với feature phái sinh có guard logic, còn categorical field được giữ rõ để sau này rà soát encoding.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Feature phái sinh chạy được trong notebook nhưng chưa tự động an toàn khi tích hợp.
* **Cách giải quyết:** Tôi thêm guard đơn giản và giữ output format ở dạng draft để nhóm rà soát.

### Nhận xét cá nhân
Tuần này làm tôi thấy feature engineering nằm ở trung tâm của AI1. Mô hình chỉ hữu ích khi field và phép biến đổi phía sau nó hợp lý. Docker/ECS cũng nhắc tôi rằng tính tái lập rất quan trọng khi code rời khỏi notebook. Tôi cẩn thận hơn trong việc ghi lại giả định, đặc biệt với feature phái sinh.
