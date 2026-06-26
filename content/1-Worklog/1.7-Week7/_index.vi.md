---
title: "Worklog Tuần 7"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 7** — 31/05/2026 – 06/06/2026
{{% /notice %}}

### Mục tiêu tuần 7:
* Tìm hiểu AWS Lambda và xây dựng cơ chế trigger tự động từ S3 sang SQS.
* Xây dựng AI Engine đọc dữ liệu từ SQS và chạy suy luận mô hình.
* Tích hợp toàn bộ luồng xử lý và ghi kết quả dự đoán vào RDS PostgreSQL.

### Các công việc đã thực hiện trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Tìm hiểu AWS Lambda và cơ chế trigger event-driven | 01/06/2026 | 01/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| Thứ 3 | - Xây dựng Lambda function tự động nhận event khi có file mới trên S3 | 02/06/2026 | 02/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| Thứ 4 | - Cấu hình Lambda đẩy dữ liệu Zeek log mới vào SQS | 03/06/2026 | 03/06/2026 | |
| Thứ 5 | - Xây dựng AI Engine đọc dữ liệu theo batch từ SQS | 04/06/2026 | 04/06/2026 | |
| Thứ 6 | - Tích hợp mô hình ONNX và ghi kết quả dự đoán vào RDS PostgreSQL | 05/06/2026 | 05/06/2026 | |

### Kết quả đạt được tuần 7:
* Triển khai thành công luồng xử lý tự động serverless luân chuyển dữ liệu từ S3 qua SQS.
* Xây dựng AI Engine hoạt động trơn tru, tiêu thụ tin nhắn từ hàng đợi một cách hiệu quả.
* Hoàn thiện mạch dữ liệu end-to-end, lưu trữ vĩnh viễn cảnh báo tấn công xuống cơ sở dữ liệu.
