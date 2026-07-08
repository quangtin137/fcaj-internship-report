---
title: "Tuần 9 - Tìm hiểu SageMaker và đánh giá mô hình dựa trên Zeek"
date: 2026-06-12
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice info %}}
**Worklog Tuần 9** - 12/06/2026 - 18/06/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần 9 tập trung vào tư duy vòng đời mô hình và đánh giá thận trọng hướng Zeek-based mới. Tôi tìm hiểu SageMaker ở mức tổng quan, sắp xếp output thử nghiệm gần nhất và xem các metric ban đầu như Recall, F1, false positive rate. Các con số hữu ích để trao đổi, nhưng vẫn chỉ ở mức thử nghiệm vì sample selection và threshold chưa chín.

### Mục tiêu trong tuần
* Ôn SageMaker và khái niệm vòng đời mô hình.
* Sắp xếp output thử nghiệm Zeek-based để dễ so sánh.
* Đánh giá Recall, F1 và false positive rate ở mức thử nghiệm.
* Rà soát độ nhạy do cách chọn mẫu và giả định threshold.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---|---|---|---|---|---|
| Ngày 1 | 12/06/2026 | 4 giờ | Tìm hiểu SageMaker như training job, model artifact và experiment tracking. | Hiểu cách dịch vụ ML managed tổ chức vòng đời mô hình. | Dự án chưa cần triển khai SageMaker đầy đủ ở thời điểm này. | Giữ phần học ở mức khái niệm và liên hệ với tài liệu. | Sắp xếp output thử nghiệm Zeek. |
| Ngày 2 | 13/06/2026 | 4 giờ | Gom kết quả thử nghiệm Zeek-based và file score. | Dễ so sánh các lần chạy hơn. | Một số tên file chưa thể hiện rõ version feature. | Bổ sung ghi chú version trong tên file hoặc report. | Tính metric ban đầu. |
| Ngày 3 | 15/06/2026 | 5 giờ | Rà soát Recall, F1 và false positive rate trên mẫu test. | Có góc nhìn đầu tiên về mô hình sau khi chuyển sang feature Zeek. | Metric thay đổi khi thành phần mẫu thay đổi. | Trình bày chúng như chỉ báo thử nghiệm. | Kiểm tra false positive. |
| Ngày 4 | 16/06/2026 | 4 giờ | Xem false positive và độ nhạy theo cách chọn mẫu. | Nhận ra một số record trông bình thường vẫn có anomaly score cao. | Chưa đủ bằng chứng để đặt threshold cuối. | Để phần threshold tuning sang tuần 10. | Cập nhật ghi chú báo cáo. |
| Ngày 5 | 17/06/2026 | 3 giờ | Cập nhật worklog/report với bài học từ dataset mismatch và đánh giá Zeek. | Liên kết được bài học cũ với hướng mới. | Cách kể cần tránh tạo cảm giác đã validation đầy đủ. | Dùng ngôn ngữ đánh giá thận trọng. | Chuyển sang CloudTrail và threshold review. |

### Chi tiết thực hiện
SageMaker được xem như tài liệu tham khảo về vòng đời mô hình: training job, artifact, version và experiment tracking. Dù chưa triển khai đầy đủ, các khái niệm này giúp tôi tổ chức output AI1 có trách nhiệm hơn.

Phần đánh giá Zeek-based chỉ dùng metric ban đầu như chỉ báo. Recall, F1 và false positive rate được xem cùng ghi chú về cách chọn mẫu để báo cáo không phóng đại chất lượng mô hình.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Metric ban đầu nhạy với cách chọn mẫu và giả định threshold.
* **Cách giải quyết:** Tôi trình bày chúng là kết quả đánh giá ở mức thử nghiệm và đặt hạn chế ngay cạnh các con số.

### Nhận xét cá nhân
Tuần này giúp tôi thận trọng hơn với metric. Một con số có vẻ khách quan, nhưng vẫn phụ thuộc vào cách chọn mẫu và chọn threshold. Khái niệm từ SageMaker cũng giúp tôi nhìn xa hơn một notebook riêng lẻ. Tôi bắt đầu xem AI1 như một phần cần có version thử nghiệm và cách diễn giải rõ ràng.
