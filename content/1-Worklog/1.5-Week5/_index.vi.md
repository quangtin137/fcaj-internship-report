---
title: "Tuần 5 - Thực hành CloudWatch và rà soát hạn chế dataset công khai"
date: 2026-05-15
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
{{% notice info %}}
**Worklog Tuần 5** - 15/05/2026 - 21/05/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần 5 chuyển trọng tâm sang monitoring và scaling, đồng thời vấn đề dataset của AI1 bắt đầu rõ hơn. Tôi tìm hiểu EC2 Auto Scaling, CloudWatch Alarm, rồi quay lại các thử nghiệm dataset công khai để xem vì sao các nhãn như Port Scan hay Brute Force không thể tự động chứng minh AI1 triển khai được.

### Mục tiêu trong tuần
* Ôn EC2 Auto Scaling và các tín hiệu dùng để scale.
* Tạo hoặc rà soát điều kiện CloudWatch Alarm.
* Tiếp tục làm sạch field dataset công khai và rà soát nhãn.
* Tài liệu hóa vì sao attack category trong dataset công khai chưa đồng nghĩa với bằng chứng AI1 có thể triển khai.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---|---|---|---|---|---|
| Ngày 1 | 15/05/2026 | 4 giờ | Tìm hiểu EC2 Auto Scaling group và scaling policy. | Hiểu cách capacity thay đổi theo điều kiện. | Dễ chỉ nhìn vào cơ chế scale mà quên liên hệ với monitoring. | Đọc scaling cùng với CloudWatch metrics. | Rà soát alarm. |
| Ngày 2 | 16/05/2026 | 4 giờ | Rà soát CloudWatch metrics và các trạng thái alarm. | Hiểu cách threshold có thể kích hoạt phản ứng vận hành. | Threshold alarm dễ cấu hình nhưng cũng dễ gây nhiễu. | Xem việc chọn threshold là phần cần có bằng chứng. | Quay lại phần nhãn dataset. |
| Ngày 3 | 18/05/2026 | 4 giờ | Tiếp tục làm sạch cột dataset công khai và kiểm tra giá trị label. | Phát hiện thêm khác biệt về cách đặt tên và độ chi tiết category. | Một số nhãn quá cụ thể so với bài toán anomaly detection không giám sát. | Tách phân tích label khỏi phần model scoring. | Đọc kỹ các nhóm tấn công. |
| Ngày 4 | 19/05/2026 | 5 giờ | Rà soát Port Scan và Brute Force như các hành vi tham khảo. | Hiểu hành vi mạng tốt hơn mà không viết là đã sinh traffic Zeek. | Nhãn công khai không đồng nghĩa với nguồn dữ liệu triển khai thật. | Dùng chúng để học, không dùng để chứng minh pipeline cuối. | Viết ghi chú hạn chế dataset. |
| Ngày 5 | 20/05/2026 | 3 giờ | Tài liệu hóa các hạn chế dataset cho báo cáo. | Có lập luận rõ hơn về việc cần chiến lược dữ liệu khác. | Phần hạn chế cần viết thận trọng. | Trình bày là observation/limitation, không xem là thất bại của dự án. | Chuẩn bị sang dịch vụ giám sát bảo mật. |

### Chi tiết thực hiện
CloudWatch và Auto Scaling cho thấy quyết định vận hành phụ thuộc vào tín hiệu đo được. Điều này cũng giống AI1: viết một threshold thì dễ, nhưng không nên tin nó nếu chưa hiểu nhiễu và bối cảnh.

Phần AI1 vẫn xoay quanh dataset công khai, đặc biệt là rà soát label và làm sạch dữ liệu. Port Scan và Brute Force được xem như nhóm hành vi để học, không phải bằng chứng rằng lab attack dựa trên Zeek đã được xây dựng.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Nhãn attack trong dataset công khai hữu ích nhưng không chuyển trực tiếp thành pipeline AI1 triển khai được.
* **Cách giải quyết:** Tôi mô tả chúng như tài liệu tham khảo hành vi và ghi rõ hạn chế triển khai trong báo cáo.

### Nhận xét cá nhân
Tuần này thay đổi cách tôi nhìn dataset công khai. Chúng có giá trị để học thuật ngữ và hành vi tấn công, nhưng không nên dùng máy móc. CloudWatch cũng giúp tôi nghĩ về false alarm và threshold từ góc nhìn vận hành. Nhờ vậy phần phân tích hạn chế của AI1 trở nên thực tế hơn, không chỉ là lý thuyết.
