---
title: "Tuần 10 - CloudTrail, rà soát threshold và Event Cloud Architect"
date: 2026-06-19
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice info %}}
**Worklog Tuần 10** - 19/06/2026 - 25/06/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần 10 kết hợp tìm hiểu audit log, hoạt động cộng đồng và rà soát threshold của AI1. Tôi học CloudTrail để hiểu record hoạt động API trên AWS, tham gia hoặc ghi chú từ Event Cloud Architect, rồi kiểm tra candidate threshold và false positive từ các thử nghiệm Zeek-based. Explainability được xem như hướng nghiên cứu offline, chưa phải tích hợp hoàn chỉnh.

### Mục tiêu trong tuần
* Tìm hiểu CloudTrail và giá trị của audit log.
* Ghi nhận bài học hữu ích từ Event Cloud Architect.
* Rà soát candidate threshold và phân phối score của AI1.
* Kiểm tra false positive và xem SHAP như một hướng explainability offline.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---|---|---|---|---|---|
| Ngày 1 | 19/06/2026 | 4 giờ | Tìm hiểu CloudTrail event và use case audit log. | Hiểu cách rà soát API action sau khi sự kiện xảy ra. | Audit log khác với network telemetry. | Xếp CloudTrail vào nhóm audit/evidence. | Chuẩn bị ghi chú event. |
| Ngày 2 | 20/06/2026 | 4 giờ | Tham gia hoặc tổng hợp ghi chú Event Cloud Architect. | Thu được một số điểm về kiến trúc và vận hành hữu ích cho báo cáo. | Ghi chú event cần ngắn gọn. | Chỉ giữ điểm liên quan đến quá trình thực tập. | Rà soát threshold AI1. |
| Ngày 3 | 22/06/2026 | 5 giờ | So sánh candidate threshold với phân phối score. | Thấy cutoff khác nhau làm số anomaly thay đổi. | Một con số chính xác dễ bị hiểu là threshold đã chốt. | Dùng cách viết candidate threshold. | Kiểm tra false positive. |
| Ngày 4 | 23/06/2026 | 4 giờ | Rà soát ví dụ false positive từ score Zeek-based. | Nhận ra một số record cần thêm context trước khi gọi là bất thường. | Score một mình chưa giải thích vì sao record đáng ngờ. | Bổ sung ghi chú false positive vào phần đánh giá. | Đọc thêm về explainability. |
| Ngày 5 | 24/06/2026 | 3 giờ | Xem SHAP như một hướng explainability offline có thể tham khảo. | Hiểu ý tưởng nhưng không viết là đã tạo SHAP plot. | Explainability rất dễ bị viết quá mức. | Chỉ mô tả là đã khảo sát/rà soát. | Chuyển sang chuẩn bị tài liệu. |

### Chi tiết thực hiện
CloudTrail bổ sung góc nhìn audit vào lộ trình học AWS. Khác với network log, CloudTrail ghi lại hoạt động API, hữu ích cho accountability và điều tra.

Với AI1, tôi so sánh candidate threshold với phân phối score và ví dụ false positive. Phần threshold được giữ ở mức thử nghiệm, còn SHAP chỉ là hướng giải thích offline có thể tham khảo, không phải tính năng dashboard đã làm xong.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Candidate threshold rất dễ bị trình bày như quyết định cuối.
* **Cách giải quyết:** Tôi đặt mỗi ghi chú threshold cạnh phần chưa chắc chắn, false-positive review và nhu cầu kiểm chứng rộng hơn.

### Nhận xét cá nhân
Tuần này cho tôi thấy threshold tuning vừa là vấn đề kỹ thuật vừa là vấn đề trình bày. Nếu báo cáo viết candidate như kết quả cuối, người đọc có thể tin hệ thống quá mức. CloudTrail cũng nhắc tôi rằng công việc bảo mật thường dựa vào bằng chứng sau sự kiện. Explainability và auditability vì vậy trở nên quan trọng hơn trong cách tôi mô tả AI1.
