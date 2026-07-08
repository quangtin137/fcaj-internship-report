---
title: "Tuần 6 - Dịch vụ giám sát bảo mật và điều chỉnh chiến lược dữ liệu"
date: 2026-05-22
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice info %}}
**Worklog Tuần 6** - 22/05/2026 - 28/05/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần 6 là lúc các hạn chế của dataset công khai chuyển thành một cuộc rà soát chiến lược dữ liệu rõ ràng hơn. Tôi học VPC Flow Logs, GuardDuty và Security Hub để hiểu monitoring bảo mật native trên AWS, rồi tổng hợp vì sao CICIDS/CICIDS2018 chưa phù hợp với AI1 theo pipeline dự kiến. Kết luận là cần chuyển sang nguồn log gần telemetry thực tế hơn.

### Mục tiêu trong tuần
* Tìm hiểu VPC Flow Logs và metadata mạng mà dịch vụ cung cấp.
* Rà soát vai trò GuardDuty và Security Hub trong monitoring bảo mật AWS.
* Tổng hợp hạn chế schema, feature và label của CICIDS/CICIDS2018.
* Phác thảo danh sách artifact AI1 dự kiến và đề xuất hướng dữ liệu tiếp theo.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---|---|---|---|---|---|
| Ngày 1 | 22/05/2026 | 4 giờ | Tìm hiểu các field và use case của VPC Flow Logs. | Biết thêm một nguồn bằng chứng network-level trong AWS. | Flow Logs không giống Zeek và có mức chi tiết khác. | So sánh dịch vụ theo thông tin thực tế. | Rà soát GuardDuty và Security Hub. |
| Ngày 2 | 23/05/2026 | 4 giờ | Đọc overview GuardDuty và Security Hub. | Hiểu cách managed finding hỗ trợ security monitoring. | Managed finding không thay thế anomaly score của AI1. | Xem chúng là nguồn bằng chứng riêng. | Tổng hợp vấn đề từ thử nghiệm dataset. |
| Ngày 3 | 25/05/2026 | 5 giờ | Gom ghi chú từ preprocessing và thử model với CICIDS/CICIDS2018. | Nhóm vấn đề thành schema mismatch, feature mismatch và khó khăn về label. | Cùng một vấn đề xuất hiện dưới nhiều dạng ở các notebook. | Viết phần hạn chế theo nguyên nhân, không theo từng notebook. | Trao đổi hướng dữ liệu. |
| Ngày 4 | 26/05/2026 | 4 giờ | Trao đổi vì sao cần nguồn log gần pipeline dự án hơn. | Có lý do rõ hơn để chuyển sang Zeek conn.log. | Việc đổi hướng cần viết như điều chỉnh kỹ thuật, không phải làm lại đột ngột. | Trình bày theo hướng giảm train-serving mismatch. | Phác thảo artifact dự kiến. |
| Ngày 5 | 27/05/2026 | 3 giờ | Phác thảo ghi chú về artifact AI1 và hướng dữ liệu dự kiến. | Chuẩn bị mốc chuyển hướng cho Tuần 7. | Artifact mới ở mức planned, chưa hoàn tất. | Dùng từ planned/expected trong báo cáo. | Bắt đầu đọc schema Zeek tuần sau. |

### Chi tiết thực hiện
VPC Flow Logs, GuardDuty và Security Hub giúp tôi tách rõ các loại bằng chứng bảo mật khác nhau. AWS có thể cung cấp network metadata và managed finding, nhưng AI1 vẫn cần một nguồn feature nhất quán để train và đánh giá.

Phần review dataset dẫn đến một lo ngại thực tế: train-serving mismatch. Mô hình học từ feature dataset công khai có thể hoạt động kém nếu lúc triển khai nhận định dạng log khác. Đây là lý do đề xuất Zeek conn.log làm hướng tiếp theo.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Rủi ro lớn nhất là train trên một định nghĩa feature nhưng serving bằng định nghĩa khác.
* **Cách giải quyết:** Tôi tổng hợp mismatch rõ ràng và viết hướng Zeek như một đề xuất/định hướng, chưa xem là migration đã hoàn tất.

### Nhận xét cá nhân
Tuần này giúp tôi hiểu rằng đổi chiến lược dữ liệu có thể là một quyết định kỹ thuật có trách nhiệm. Phần làm với dataset công khai không bị lãng phí; nó chỉ ra điều gì có thể sai nếu bỏ qua dữ liệu lúc serving. Các dịch vụ bảo mật AWS cũng cho thấy nền tảng monitoring kết hợp nhiều loại bằng chứng. AI1 cần khớp vào hệ đó, thay vì nghĩ một dataset có thể giải quyết mọi thứ.
