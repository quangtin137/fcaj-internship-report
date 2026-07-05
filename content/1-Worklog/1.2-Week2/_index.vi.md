---
title: "Tuần 2 - Thiết lập lab Zeek và nghiên cứu schema conn.log"
date: 2026-04-24
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 2** — 24/04/2026 – 30/04/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần này, tôi tập trung vào khía cạnh hạ tầng cần thiết để sinh ra nguồn ML telemetry chính của chúng tôi. Tôi đã nghiên cứu cấu hình lab giám sát bảo mật và phân tích sâu schema của Zeek `conn.log` để hiểu rõ các đặc trưng mạng thô mà mình sẽ xử lý.

### Mục tiêu trong tuần
* Nắm bắt môi trường lab giám sát bảo mật trên AWS (VPC, EC2).
* Nghiên cứu schema của Zeek `conn.log` và phân tích `conn_dataset`.
* Thiết kế schema đặc trưng ban đầu cho mô hình tầng network AI1.
* Phân biệt rõ sự khác nhau giữa raw log, processed feature và model input.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---:|---|---|---|---|---|
| Ngày 1 | 24/04/2026 | 5 giờ | Xem lại cấu hình VPC và EC2 của lab để hiểu Zeek capture traffic ở đâu. | Nắm được topology mạng đang sinh ra dữ liệu. | Cần tách biệt log Zeek khỏi các log hệ thống thông thường. | Chỉ tập trung vào output `conn.log` của Zeek trên S3 bucket. | Nghiên cứu các trường của `conn.log`. |
| Ngày 2 | 25/04/2026 | 4 giờ | Đọc tài liệu Zeek về các trường `conn.log` (duration, orig_bytes, resp_bytes, conn_state). | Xác định được các flow-level features nền tảng. | Một số trường như `conn_state` mang giá trị phân loại (categorical). | Lên kế hoạch mã hóa các trạng thái kết nối này trong feature extractor. | Phân tích raw `conn_dataset`. |
| Ngày 3 | 27/04/2026 | 6 giờ | Khám phá một `conn_dataset` mẫu để xem phân phối giá trị thực tế. | Nhận thấy giá trị thiếu (missing value) được biểu diễn bằng dấu gạch nối ('-'). | Giá trị thiếu sẽ làm hỏng tiến trình ML inference. | Đặt policy `fail_if_missing` cho các trường bắt buộc, nhưng phải viết hàm parse dấu gạch nối cẩn thận. | Phác thảo schema đặc trưng cho AI1. |
| Ngày 4 | 28/04/2026 | 4 giờ | Phác thảo schema đặc trưng AI1, tách biệt các flow feature cơ bản khỏi các cờ giao thức. | Tạo được cấu trúc rõ ràng cho feature extractor. | Rủi ro bất đồng bộ schema giữa môi trường train và serving. | Đảm bảo code feature extractor phải được dùng chung nghiêm ngặt giữa pipeline huấn luyện và inference. | Chuẩn bị thu thập normal traffic. |

### Chi tiết thực hiện
Tôi đã nghiên cứu môi trường lab bảo mật, làm quen với cách cấu hình VPC và EC2 hỗ trợ pipeline thu thập log. Công việc kỹ thuật chính của tôi là mổ xẻ cấu trúc Zeek `conn.log`. Tôi bắt đầu quan sát các đặc trưng luồng cơ bản như `duration`, `orig_bytes`, `resp_bytes`, `orig_pkts` và `resp_pkts`. Tôi cũng chú ý đến các cờ giao thức (`proto`, `service`) và trạng thái kết nối (`conn_state`). Tôi sớm nhận ra rằng phải xây dựng một bộ feature extractor vững chắc để parse các raw log này một cách nhất quán, bởi vì việc giữ tính đồng nhất của schema là cách duy nhất để tránh hiện tượng train-serving mismatch khi đưa mô hình ra thực tế.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Tôi nhận ra các file `conn.log` thô không thể dùng ngay cho machine learning do chứa các giá trị thiếu (thường ký hiệu bằng `-`) và chuỗi ký tự phân loại.
* **Cách giải quyết:** Tôi quyết định feature extractor phải xử lý tường minh những vấn đề này. Tôi hình thành tư duy nghiêm ngặt về schema: raw log phải được biến đổi một cách tất định thành một vector số có độ dài cố định trước khi đưa vào mô hình, tạo tiền đề cho file `feature_manifest.json` mà tôi sẽ tạo sau này.

### Nhận xét cá nhân
Việc đào sâu vào các file log Zeek thô khiến dự án trở nên thực tế hơn rất nhiều. Tôi nhận ra một mô hình machine learning phụ thuộc hoàn toàn vào chất lượng và cấu trúc của data pipeline. Nắm được cách hạ tầng AWS VPC chủ động định tuyến traffic về các Zeek sensor giúp tôi đánh giá cao mối liên kết chặt chẽ giữa mạng cloud và giám sát bảo mật bằng AI.
