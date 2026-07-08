---
title: "Tuần 11 - Rà soát Well-Architected và tài liệu kỹ thuật AI1"
date: 2026-06-26
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice info %}}
**Worklog Tuần 11** - 26/06/2026 - 02/07/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần 11 chuyển từ thử nghiệm sang giải thích và tài liệu hóa. Tôi rà soát AWS Well-Architected Framework, ghi nhận hoạt động cộng đồng nếu có liên quan, rồi soạn tài liệu kỹ thuật AI1. Mục tiêu chính là mô tả expected data flow, giả định input/output, preprocessing dựa trên Zeek và giới hạn mô hình mà không khẳng định đã kiểm thử end-to-end hoàn chỉnh.

### Mục tiêu trong tuần
* Rà soát các pillar Well-Architected liên quan reliability, security và cost.
* Soạn tài liệu kỹ thuật AI1 cho website report.
* Tài liệu hóa giả định input/output và hạn chế preprocessing Zeek.
* Chuẩn bị mô tả demo flow bằng cách viết expected data flow.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---|---|---|---|---|---|
| Ngày 1 | 26/06/2026 | 4 giờ | Tìm hiểu các pillar của AWS Well-Architected Framework. | Liên hệ được reliability, security và cost với dự án thực tập. | Framework khá rộng nên dễ tóm tắt quá chung. | Chỉ chọn điểm liên quan đến báo cáo này. | Rà soát ghi chú event/cộng đồng. |
| Ngày 2 | 27/06/2026 | 4 giờ | Ghi lại điểm hữu ích từ Community Day hoặc trao đổi nhóm. | Có thêm vài nhắc nhở thực tế về kiến trúc. | Không phải chi tiết event nào cũng cần đưa vào worklog. | Giữ phần event ngắn gọn. | Soạn tài liệu AI1. |
| Ngày 3 | 29/06/2026 | 5 giờ | Soạn tài liệu kỹ thuật AI1: phạm vi, input field, preprocessing và ý tưởng output. | Có phần giải thích rõ hơn cho người đọc không trực tiếp làm AI1. | Một số giả định mới ở mức expected, chưa validation đầy đủ. | Gắn nhãn rõ các giả định trong tài liệu. | Bổ sung hạn chế. |
| Ngày 4 | 30/06/2026 | 4 giờ | Tài liệu hóa hạn chế về feature extraction, sample size và threshold review. | Phần AI1 dễ bảo vệ hơn khi trình bày. | Dễ muốn viết câu chuyện tích hợp mạnh hơn thực tế. | Tránh khẳng định full end-to-end. | Chuẩn bị mô tả demo flow. |
| Ngày 5 | 01/07/2026 | 3 giờ | Chuẩn bị ghi chú expected demo-flow và nội dung website. | Các trang report khớp hơn với trạng thái hiện tại của dự án. | Luồng cần dễ hiểu nhưng không overpromise. | Dùng từ prepared/reviewed cho bước tích hợp. | Chuyển sang rà soát website cuối. |

### Chi tiết thực hiện
Well-Architected giúp tôi nhìn dự án rộng hơn phần code. Các yếu tố reliability, security và cost hữu ích khi viết về dịch vụ AWS và các quyết định trong báo cáo.

Tài liệu AI1 tập trung vào sự rõ ràng: input dự kiến là gì, preprocessing dựa trên Zeek được mô tả ra sao, mô hình cố gắng phát hiện điều gì và giới hạn còn lại ở đâu. Demo flow được viết là prepared/expected, không phải đường production đã validation đầy đủ.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Tài liệu rất dễ nghe mạnh hơn trạng thái tích hợp thực tế.
* **Cách giải quyết:** Tôi ghi trực tiếp assumption và limitation trong phần tài liệu kỹ thuật AI1.

### Nhận xét cá nhân
Tuần này giúp tôi thấy tài liệu cũng là một phần của engineering. Một mô hình không được giải thích rõ sẽ khó để đồng đội sử dụng hoặc review. Viết limitation ban đầu hơi khó, nhưng làm báo cáo trung thực hơn. Tôi cũng học được rằng framework kiến trúc chỉ hữu ích khi dẫn đến quyết định cụ thể, không phải khi chép lại chung chung.
