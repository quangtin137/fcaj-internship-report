---
title: "Tuần 2 - Thực hành AWS VPC/EC2 và khảo sát dataset công khai"
date: 2026-04-24
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
{{% notice info %}}
**Worklog Tuần 2** - 24/04/2026 - 30/04/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần 2 chuyển từ onboarding sang thực hành networking cơ bản trên AWS. Tôi ôn VPC, subnet, route table, security group, EC2, SSH và IAM Role, đồng thời bắt đầu khảo sát dataset an ninh mạng công khai. Phần AI1 vẫn ở mức tìm hiểu, chủ yếu đọc metadata, nhãn và ý nghĩa feature của CICIDS/CICIDS2018.

### Mục tiêu trong tuần
* Ôn quan hệ giữa VPC, subnet, route table và security group.
* Tạo hoặc rà soát EC2 instance và kết nối SSH an toàn.
* Đọc mô tả CICIDS/CICIDS2018, nhãn và nhóm tấn công.
* Lập ghi chú so sánh ban đầu giữa feature dataset công khai và nhu cầu anomaly detection của AI1.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---|---|---|---|---|---|
| Ngày 1 | 24/04/2026 | 4 giờ | Ôn VPC, subnet, route table và security group. | Hiểu hơn cách AWS biểu diễn cô lập mạng. | Ban đầu tôi dễ nhầm giữa route table và subnet association. | Vẽ sơ đồ VPC nhỏ trước khi chỉnh cấu hình. | Thực hành truy cập EC2. |
| Ngày 2 | 25/04/2026 | 4 giờ | Tạo hoặc rà soát EC2 instance và kiểm tra yêu cầu SSH. | Liên hệ được lý thuyết networking với một tài nguyên compute cụ thể. | Inbound rule của security group cần kiểm tra kỹ. | Chỉ mở quyền SSH cần thiết cho phạm vi lab. | Chuyển sang đọc dataset công khai. |
| Ngày 3 | 27/04/2026 | 5 giờ | Đọc metadata, mô tả label và nhóm tấn công của CICIDS/CICIDS2018. | Có hình dung ban đầu về cấu trúc dataset. | Dataset có nhiều flow feature đã được tính sẵn nhưng nguồn gốc không phải lúc nào cũng rõ. | Ghi lại field chưa rõ thay vì tự giả định nó khớp pipeline dự án. | So sánh feature với nhu cầu AI1. |
| Ngày 4 | 28/04/2026 | 4 giờ | So sánh feature dataset công khai với bài toán phát hiện bất thường mạng. | Nhận ra label công khai hữu ích để học nhưng chưa chắc phù hợp với log khi triển khai. | Định nghĩa feature khác với hướng log-based mà dự án có thể dùng về sau. | Giữ dataset công khai ở vai trò khảo sát, chưa xem là input cuối. | Tuần sau tiếp tục với lưu trữ và tiền xử lý. |

### Chi tiết thực hiện
Phần AWS tập trung vào cách hình thành một mạng cloud nhỏ. VPC và subnet cho tôi hiểu ranh giới môi trường, route table giải thích hướng đi của traffic, còn security group thể hiện cách kiểm soát truy cập quanh EC2.

Với AI1, tôi bắt đầu từ tài liệu dataset thay vì train model ngay. CICIDS/CICIDS2018 là ví dụ hữu ích về dữ liệu security dạng flow, nhưng tôi cũng thấy rằng các field của dataset công khai không tự động tương thích với pipeline dựa trên log sau này.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Tài liệu dataset công khai có nhiều feature, nhưng không phải feature nào cũng khớp rõ với luồng dữ liệu dự kiến của dự án.
* **Cách giải quyết:** Tôi lập ghi chú so sánh và đánh dấu các field chưa chắc chắn để rà soát sau, không ép schema khớp ngay.

### Nhận xét cá nhân
Tuần này làm cho networking trên cloud bớt trừu tượng hơn với tôi. Việc truy cập EC2 phụ thuộc vào thiết kế VPC, routing và security rule, nên security monitoring không thể tách khỏi kiến thức hạ tầng. Ở phần AI1, tôi học được cách đọc tài liệu dataset thận trọng hơn. Một dataset có thể rất nhiều feature nhưng vẫn khó dùng nếu không khớp bối cảnh triển khai.
