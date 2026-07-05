---
title: "Tuần 11 - Kiểm thử end-to-end và tài liệu kỹ thuật AI1"
date: 2026-06-26
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 11** — 26/06/2026 – 02/07/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần này xoay quanh công tác kiểm duyệt gắt gao và tài liệu hóa. Tôi đã tiến hành test end-to-end toàn bộ vòng đời dữ liệu chạy dọc theo pipeline AWS, đồng thời hoàn thiện bộ tài liệu kỹ thuật chi tiết cho module AI1, định nghĩa cực kỳ nghiêm ngặt các hợp đồng input và output.

### Mục tiêu trong tuần
* Chạy kiểm thử end-to-end pipeline: Zeek -> S3 -> SQS -> AI1 -> Fusion Layer -> Dashboard.
* Soạn thảo tài liệu kỹ thuật AI1, ghi rõ chính xác hợp đồng input và output.
* Tài liệu hóa các hạn chế (limitations) của mô hình, bao gồm sự phụ thuộc vào schema Zeek và normal baseline.
* Chuẩn bị các kịch bản chuẩn bị cho buổi demo live attack cuối kỳ.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---:|---|---|---|---|---|
| Ngày 1 | 26/06/2026 | 5 giờ | Chạy test end-to-end bằng cách bắn Nmap scan vào các EC2 lab. | Trace thành công luồng dữ liệu từ lúc sinh `conn.log`, qua SQS batch polling, vào AI1 adapter, và sang tới Fusion Layer. | Dashboard hiển thị hơi có độ trễ (delay). | Tối ưu hóa tần suất batch polling để giảm độ trễ trước khi demo. | Viết tài liệu hợp đồng AI1. |
| Ngày 2 | 27/06/2026 | 4 giờ | Tham gia FCAJ Community Day - Data Driven, AI Risen. | Có thêm góc nhìn về cách AI agent hỗ trợ vận hành doanh nghiệp và security workflow. | Cần tách riêng insight cấp cao từ event với phần triển khai thực tế của AI1. | Đưa bài thu hoạch chi tiết vào Event Participated, trong Worklog chỉ dùng các bài học liên quan. | Áp dụng tư duy AI-driven operations khi review pipeline end-to-end. |
| Ngày 3 | 29/06/2026 | 5 giờ | Soạn tài liệu kỹ thuật AI1, chi tiết hóa `feature_manifest.json` và yêu cầu schema 30-feature. | Lập ra một input contract rõ ràng cho bất kỳ kỹ sư nào làm việc với AI1 sau này. | Cần định nghĩa rõ cách xử lý lỗi. | Nhấn mạnh policy `fail_if_missing` là một tính năng bảo vệ toàn vẹn dữ liệu chứ không phải lỗi (bug). | Viết output contract. |
| Ngày 4 | 30/06/2026 | 4 giờ | Tài liệu hóa output contract: `label`, `confidence`, và `selected_threshold` (0.398066). | Định nghĩa rõ cách Fusion Layer phải biên dịch mức điểm `higher_is_more_anomalous`. | Không có vấn đề lớn. | Tiến hành tài liệu hóa các hạn chế (limitations) đã biết. | Phác thảo phần limitation của AI1. |
| Ngày 5 | 01/07/2026 | 4 giờ | Ghi chép giới hạn của AI1 về việc phụ thuộc chặt vào bản cập nhật Zeek và yêu cầu phải retrain nếu topology baseline thay đổi lớn. | Một bản review minh bạch về các giới hạn vận hành của mô hình. | Không có vấn đề lớn. | Chuẩn bị kịch bản demo. | Chốt công tác chuẩn bị demo. |

### Chi tiết thực hiện
Tôi dồn toàn lực vào kiểm thử end-to-end, thực hiện bắn các lệnh tấn công thật và theo dõi sát sao dấu vết log đi qua SQS queue, vào Feature Router, qua AI1 inference adapter, và kết thúc ở Fusion Layer để hiển thị lên RDS/Dashboard. Khi pipeline đã chứng minh được tính ổn định, tôi chắp bút viết Tài Liệu Kỹ Thuật AI1. Tôi định nghĩa dứt khoát hợp đồng đầu vào (input contract): mô hình bắt buộc nhận một vector số gồm đúng 30 đặc trưng khớp hoàn toàn với `feature_manifest.json` sinh ra từ Zeek `conn.log`, áp đặt cứng chính sách `fail_if_missing`. Tôi cũng phác thảo hợp đồng đầu ra (output contract), chỉ rõ các service phía sau sẽ nhận một JSON payload chứa `label` (NORMAL/ANOMALY), `confidence`, và `selected_threshold`.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Việc trình bày các hạn chế (limitations) của AI1 sao cho trung thực mà không làm hạ thấp giá trị công sức mình bỏ ra là một nghệ thuật viết tài liệu khá tinh tế.
* **Cách giải quyết:** Tôi trình bày các hạn chế này dưới góc độ là "điều kiện tiên quyết để vận hành". Ví dụ, thay vì nói "mô hình sẽ lỗi nếu mạng thay đổi", tôi viết "AI1 giả định một topology lab baseline tĩnh; những thay đổi kiến trúc lớn yêu cầu phải thiết lập lại normal baseline để ngăn chặn việc tăng False Positive Rate." Cách hành văn này cung cấp chỉ dẫn rõ ràng cho những người bảo trì (maintainer) tương lai.

### Nhận xét cá nhân
Sự kiện FCAJ Community Day đã làm nổi bật tương lai của vận hành bằng AI (AI-driven operations), khiến tôi càng trân trọng hơn hệ thống tự động hóa mà chúng tôi đang xây dựng. Việc viết tài liệu kỹ thuật buộc tôi phải đặt mình vào vị trí của một lập trình viên tương lai tiếp nhận dự án này. Đảm bảo các hợp đồng (contract) luôn nghiêm ngặt và các hạn chế (limitation) phải minh bạch chính là dấu ấn của kỹ nghệ phần mềm chuyên nghiệp, vượt xa việc chỉ viết một script machine learning đơn thuần.
