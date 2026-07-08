---
title: "Tuần 7 - Chuyển hướng sang Zeek conn.log và thiết kế lại tiền xử lý AI1"
date: 2026-05-29
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice info %}}
**Worklog Tuần 7** - 29/05/2026 - 04/06/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần 7 là mốc chuyển thật sự từ khảo sát dataset công khai sang Zeek conn.log. Song song với việc học Lambda và event-driven, tôi bắt đầu đọc schema Zeek và xác định các field có thể dùng cho preprocessing AI1. Trọng tâm là thiết kế lại đường đi dữ liệu, chưa phải tích hợp xong inference pipeline.

### Mục tiêu trong tuần
* Ôn Lambda và khái niệm event-driven processing.
* Đọc cấu trúc Zeek conn.log và ý nghĩa các field thường gặp.
* Xác định field có thể dùng cho AI1 như duration, orig_bytes, resp_bytes, proto, service, conn_state.
* Định nghĩa bước đầu cách xử lý missing value và categorical field.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---|---|---|---|---|---|
| Ngày 1 | 29/05/2026 | 4 giờ | Học Lambda và các mẫu thiết kế event-driven. | Hiểu vì sao function nhỏ có thể phản ứng theo event trong pipeline. | Tuần này chưa phải lúc nối AI1 vào event thật. | Giữ phần Lambda ở mức khái niệm. | Bắt đầu đọc schema Zeek. |
| Ngày 2 | 30/05/2026 | 5 giờ | Đọc tài liệu Zeek conn.log và một số dòng mẫu. | Nhận ra các field liên quan nhất đến hành vi connection-level. | Schema khác nhiều so với cột của dataset công khai. | Thiết kế preprocessing mới thay vì dùng lại giả định từ dataset công khai. | Kiểm tra field numeric và categorical chính. |
| Ngày 3 | 01/06/2026 | 4 giờ | Rà soát duration, orig_bytes, resp_bytes, proto, service và conn_state. | Tách được nhóm feature số và categorical. | Một số giá trị xuất hiện dưới dạng '-' thay vì missing value thông thường. | Xử lý '-' rõ ràng trong bước parse. | Phác thảo rule cho missing value. |
| Ngày 4 | 02/06/2026 | 4 giờ | Định nghĩa ghi chú ban đầu cho missing value và categorical encoding. | Có điểm bắt đầu thực tế hơn cho preprocessing dựa trên Zeek. | Xử lý categorical không nhất quán có thể gây train-serving mismatch. | Giữ rule transform đơn giản và có tài liệu. | Chuẩn bị kế hoạch feature extractor. |
| Ngày 5 | 03/06/2026 | 3 giờ | Phác thảo kế hoạch preprocessing AI1 dựa trên Zeek. | Có task cụ thể cho feature extraction ở Tuần 8. | Kế hoạch vẫn cần kiểm chứng bằng sample thật. | Đánh dấu đây là draft design. | Bắt đầu làm feature extraction. |

### Chi tiết thực hiện
Lambda được học như một phần của kiến trúc event-driven, nhưng tuần này không khẳng định đã tích hợp SQS đến AI1. Giá trị chính là hiểu cách các thành phần tương lai có thể được trigger khi data contract rõ hơn.

Phần Zeek tập trung vào parse và lập kế hoạch feature. Các field như duration và byte count có thể trở thành input số, còn proto, service và conn_state cần xử lý categorical. Giá trị '-' được ghi nhận ngay từ đầu như một vấn đề tiền xử lý.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Zeek dùng '-' và nhiều categorical field, rất dễ xử lý sai trong preprocessing.
* **Cách giải quyết:** Tôi viết rule parse rõ ràng và chưa kết luận về model cho đến khi feature extractor được thử nghiệm.

### Nhận xét cá nhân
Việc chuyển sang Zeek làm pipeline AI1 có cảm giác gần hệ thống thực tế hơn. Thay vì ép dự án theo dataset công khai, tôi bắt đầu điều chỉnh preprocessing theo loại log mà dự án dự kiến dùng. Điều này cũng làm công việc khó hơn vì mỗi field cần được hiểu rõ. Tôi học được rằng nguồn dữ liệu phù hợp hơn vẫn cần engineering cẩn thận trước khi train model.
