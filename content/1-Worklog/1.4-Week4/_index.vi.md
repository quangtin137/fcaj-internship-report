---
title: "Tuần 4 - SQS Messaging và thử nghiệm Isolation Forest ban đầu"
date: 2026-05-08
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
{{% notice info %}}
**Worklog Tuần 4** - 08/05/2026 - 14/05/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần 4 kết hợp thực hành messaging trên AWS với các lần thử Isolation Forest đầu tiên. Tôi học cách SQS queue tách producer và consumer, thực hành gửi/nhận message bằng boto3, rồi dùng một subset dataset công khai để quan sát anomaly score. Các thử nghiệm này có giá trị học tập, nhưng chưa được xem là baseline ổn định của AI1.

### Mục tiêu trong tuần
* Hiểu cách SQS queue hoạt động và thao tác message cơ bản bằng boto3.
* Ghi nhận các điểm hữu ích từ FCAJ Community Day nếu có.
* Chạy thử Isolation Forest trên một số feature dataset công khai.
* Quan sát contamination và phân phối anomaly score mà không kết luận quá mức.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---|---|---|---|---|---|
| Ngày 1 | 08/05/2026 | 4 giờ | Tìm hiểu SQS queue và vòng đời message. | Hiểu vì sao queue hữu ích giữa phần ingest và xử lý. | Visibility timeout và thao tác delete message cần đọc kỹ hơn. | Ghi chú SQS theo hướng decoupling, chưa xem là tích hợp AI1 cuối. | Thử gửi/nhận message bằng boto3. |
| Ngày 2 | 09/05/2026 | 4 giờ | Thực hành boto3 với send_message và receive_message. | Nắm được dạng giao tiếp cơ bản qua queue. | Credential local và region cần kiểm tra lại. | Ghi lại giả định setup cho các lab sau. | Chuẩn bị input thử nghiệm model. |
| Ngày 3 | 11/05/2026 | 4 giờ | Chuẩn bị subset dataset công khai cho lần chạy Isolation Forest đầu. | Tạo được ma trận feature số ở quy mô nhỏ. | Chọn feature khác nhau làm phân phối input thay đổi rõ. | Chỉ dùng subset để quan sát hành vi mô hình. | Chạy Isolation Forest với vài giá trị contamination. |
| Ngày 4 | 12/05/2026 | 5 giờ | Chạy thử Isolation Forest và xem anomaly score. | Thấy contamination ảnh hưởng số dòng bị xem là anomaly. | Score không thể hiểu đơn giản như xác suất tấn công. | Mô tả score là chỉ báo bất thường, không phải confidence phân loại attack. | Viết lại kết quả khảo sát. |
| Ngày 5 | 13/05/2026 | 3 giờ | Tổng hợp vì sao thử nghiệm model chỉ mang tính exploratory. | Làm rõ dataset công khai được dùng để học hành vi mô hình. | Dễ bị hấp dẫn bởi việc gọi lần chạy đầu là baseline. | Không đặt tên nó như một phiên bản AI1 đã ổn định hoặc đóng băng. | Tiếp tục phân tích hạn chế ở tuần sau. |

### Chi tiết thực hiện
SQS cho tôi một cách nghĩ khác về AWS: producer và consumer không nhất thiết chạy cùng thời điểm. Thực hành boto3 giúp tôi thấy message payload về sau cần có cấu trúc và kiểm tra rõ ràng.

Thử nghiệm Isolation Forest được giữ ở quy mô nhỏ. Tôi chọn một số feature dạng số, chạy model và xem phân phối score thay đổi theo contamination. Kết quả là artifact học tập, không phải contract model cuối.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Isolation Forest nhạy với feature selection và contamination.
* **Cách giải quyết:** Tôi so sánh phân phối score thay vì trình bày lần chạy đầu như baseline đáng tin cậy.

### Nhận xét cá nhân
Tuần này là lần đầu phần AI1 có cảm giác rõ như một bài toán mô hình, nhưng cũng cho thấy kết quả model ban đầu rất dễ gây hiểu nhầm. Isolation Forest hữu ích để học hành vi bất thường, nhưng phụ thuộc mạnh vào cách biểu diễn input. SQS cũng nhắc tôi rằng output của model sau này phải đi qua các thành phần phần mềm. Một thử nghiệm model riêng lẻ chưa đủ để định nghĩa cả hệ thống.
