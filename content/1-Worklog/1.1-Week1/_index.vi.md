---
title: "Tuần 1 - Onboarding thực tập và phân tích phạm vi AI1"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
{{% notice info %}}
**Worklog Tuần 1** - 17/04/2026 - 23/04/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần đầu tiên chủ yếu là giai đoạn làm quen với nhịp thực tập: tìm hiểu môi trường tài khoản AWS, ôn IAM và Budgets, đồng thời đọc đề cương dự án để xác định phần việc AI1 của mình. Ở thời điểm này, Zeek chỉ được xem như một hướng telemetry có thể dùng trong kiến trúc, nên tôi tập trung làm rõ phạm vi AI1 thay vì vội xem đó là dataset chính.

### Mục tiêu trong tuần
* Hoàn tất onboarding và rà soát cách sử dụng tài khoản AWS an toàn qua IAM, Budgets.
* Đọc đề cương Hybrid IDS và xác định AI1 nằm ở đâu trong kiến trúc.
* Tách rõ phạm vi phát hiện bất thường của AI1 với AI2A, AI2B và Suricata.
* Ghi lại các câu hỏi còn mở về nguồn dữ liệu và hướng mô hình cho các tuần sau.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---|---|---|---|---|---|
| Ngày 1 | 17/04/2026 | 3 giờ | Hoàn tất onboarding, kiểm tra quyền truy cập tài khoản AWS và ôn lại IAM/Budgets. | Nắm được ranh giới sử dụng tài khoản và yêu cầu kiểm soát chi phí. | Tôi chưa quen với cách nhóm đặt tên và phân quyền trong AWS. | Giữ một checklist ngắn về an toàn tài khoản trước khi làm lab. | Đọc kỹ hơn đề cương dự án. |
| Ngày 2 | 18/04/2026 | 4 giờ | Đọc đề cương Hybrid IDS và đánh dấu các phần liên quan đến AI1. | Xác định AI1 là thành phần phát hiện bất thường ở tầng network. | Đề cương nhắc đến nhiều nguồn bằng chứng nên vai trò từng AI ban đầu chưa thật rõ. | Xem AI1 như module anomaly detection không giám sát trong khi nguồn dữ liệu còn cần khảo sát. | Trao đổi với nhóm để làm rõ ranh giới AI1. |
| Ngày 3 | 20/04/2026 | 4 giờ | Trao đổi với nhóm về vai trò AI1, AI2A, AI2B và Suricata. | Giảm nhầm lẫn giữa phát hiện bất thường bằng ML và cảnh báo rule-based. | Suricata alert ban đầu có vẻ gần với output của AI1. | AI1 không nên lặp lại Suricata mà cần cung cấp một tín hiệu anomaly riêng. | Chuẩn bị ghi chú về các nguồn telemetry có thể dùng. |
| Ngày 4 | 21/04/2026 | 4 giờ | Tổng hợp phạm vi AI1, ghi chú onboarding AWS và các câu hỏi về dataset. | Có điểm xuất phát rõ hơn cho phần khảo sát ở Tuần 2. | Hướng dataset cuối cùng chưa thể xác nhận ngay. | Viết báo cáo thận trọng và chỉ xem Zeek là một hướng tiềm năng. | Bắt đầu thực hành networking AWS và khảo sát dataset. |

### Chi tiết thực hiện
Ở phần AWS, tôi ôn IAM user/role, phạm vi quyền ở mức cơ bản và Budgets như một lớp kiểm soát chi phí khi thực hành. Việc này cần thiết vì các lab sau đều yêu cầu hiểu rõ mình được tạo, sửa hoặc xóa tài nguyên nào.

Ở phần AI1, trọng tâm chưa phải triển khai mà là đọc đề cương và khoanh vùng trách nhiệm. Kết quả chính của tuần là ghi chú phạm vi: AI1 cung cấp bằng chứng anomaly ở tầng network, còn AI2A, AI2B và Suricata xử lý các loại bằng chứng khác. Tôi tránh viết rằng Zeek conn.log đã là dataset chính ngay từ đầu.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Ranh giới AI1 dễ bị lẫn với Suricata và các module AI khác.
* **Cách giải quyết:** Tôi trao đổi lại với nhóm, ghi nhận AI1 là thành phần anomaly detection và nguồn dữ liệu vẫn cần được kiểm chứng thêm.

### Nhận xét cá nhân
Tuần này giúp tôi hiểu rằng xác định đúng vai trò cũng là một phần của công việc kỹ thuật, không chỉ là thủ tục ban đầu. Nếu mô tả AI1 quá rộng, các bước xây dựng mô hình sau này sẽ khó giải thích. Tôi cũng nhận ra tài liệu đề cương chỉ là điểm xuất phát, chưa phải bằng chứng triển khai cuối cùng. Nhờ vậy tôi thận trọng hơn khi viết worklog ngay từ đầu.
