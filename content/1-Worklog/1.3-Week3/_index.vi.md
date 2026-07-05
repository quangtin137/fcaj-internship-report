---
title: "Tuần 3 - Thu normal traffic và trích xuất đặc trưng"
date: 2026-05-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 3** — 01/05/2026 – 07/05/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần này tập trung dày đặc vào công tác data engineering. Tôi đã tiến hành thu thập lưu lượng mạng bình thường (normal traffic) để xây dựng một baseline chuẩn xác, đồng thời viết code trích xuất đặc trưng nhằm chuyển đổi raw log Zeek thành vector 30-feature dùng cho mô hình AI1.

### Mục tiêu trong tuần
* Thu thập normal network traffic để thiết lập normal baseline.
* Xây dựng bộ feature extractor cho Zeek `conn.log`.
* Xử lý missing value và chuyển đổi kiểu dữ liệu.
* Tạo các đặc trưng phái sinh (derived features) và chuẩn bị input schema chính xác cho Isolation Forest.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---:|---|---|---|---|---|
| Ngày 1 | 01/05/2026 | 5 giờ | Phối hợp cùng team thu thập normal baseline traffic đa dạng trong lab. | Thu được một `conn_dataset` sạch, không có nhiễu tấn công. | Baseline ban đầu thiếu sự đa dạng về các loại dịch vụ. | Bổ sung thêm các hoạt động lướt web HTTP, DNS và phiên SSH hợp lệ để làm phong phú baseline. | Bắt đầu viết code feature extractor. |
| Ngày 2 | 02/05/2026 | 6 giờ | Viết script Python để parse các flow feature cơ bản và xử lý missing value `-`. | Ép kiểu thành công các trường log text sang dạng số. | Cần tính toán các đặc trưng rate/ratio để nắm bắt hành vi theo thời gian. | Viết logic cho `bytes_per_pkt`, `packet_rate`, và `log1p_duration`. | Thêm các cờ giao thức và trạng thái. |
| Ngày 3 | 04/05/2026 | 5 giờ | Lập trình các cờ trạng thái kết nối (`is_success_state`, `is_failed_like_state`, `has_rst`) và cờ giao thức. | Chuyển đổi thành công các giá trị categorical thành binary features. | Cần cách bắt các hành vi giống brute-force mà không lưu state phức tạp. | Thêm các đặc trưng gộp theo source như `flow_count_per_src` và `unique_dst_ports`. | Thêm đặc trưng SSH rolling-window. |
| Ngày 4 | 05/05/2026 | 5 giờ | Phát triển các đặc trưng SSH rolling-window (vd: `ssh_count_60s_same_src`, `ssh_non_success_60s`). | Hoàn thiện tập hợp các đặc trưng phái sinh phức tạp. | Việc theo dõi cửa sổ 60s yêu cầu xử lý có trạng thái (stateful). | Sử dụng phép toán rolling window của pandas trong lúc chuẩn bị dữ liệu huấn luyện. | Chốt lại 30-feature schema. |
| Ngày 5 | 06/05/2026 | 4 giờ | Chốt schema đầu vào 30-feature cho AI1. | Có một feature vector hoàn chỉnh, độ dài cố định sẵn sàng cho mô hình. | Không có vấn đề lớn. | Tiến hành viết tài liệu mô tả logic schema. | Chuẩn bị huấn luyện mô hình. |

### Chi tiết thực hiện
Tôi đã xây dựng bộ feature extractor để biến đổi `conn.log` thô thành vector 30-feature đã định. Tôi xử lý các giá trị thiếu bằng cách thay dấu gạch nối của Zeek thành null hoặc 0 tùy trường hợp. Bên cạnh các flow feature cơ bản (`duration`, `orig_bytes`, `resp_bytes`), tôi tạo ra các đặc trưng tỷ lệ (rate/ratio) như `bytes_per_pkt`, `packet_rate` và `orig_resp_byte_ratio` để nắm bắt bối cảnh hành vi. Dữ liệu phân loại được mã hóa thành các cờ (`is_tcp`, `is_ssh`, `is_success_state`, `is_failed_like_state`). Để cung cấp ngữ cảnh cho các cuộc tấn công như Port Scan hay Brute Force, tôi tổng hợp các đặc trưng đích/nguồn (`flow_count_per_src`, `unique_dst_ports`) và lập trình các đặc trưng cửa sổ trượt (rolling-window) quan trọng cho SSH (`ssh_count_60s_same_src`, `ssh_non_success_60s`).

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Đảm bảo normal baseline đủ bao quát là một thách thức. Nếu mô hình chỉ học các traffic chạy ngầm tĩnh lặng, bất kỳ hoạt động bình thường nào của user cũng sẽ gây ra cảnh báo sai (false positive).
* **Cách giải quyết:** Tôi chủ động sinh các lưu lượng bình thường có kèm việc download file nặng qua HTTP, truy vấn DNS liên tục, và các phiên admin SSH hợp lệ. Baseline đa dạng này ngăn chặn việc mô hình học một định nghĩa "bình thường" quá hẹp.

### Nhận xét cá nhân
Viết logic trích xuất đặc trưng khá khó nhằn nhưng đem lại thành quả xứng đáng. Rõ ràng là trí thông minh của AI1 không chỉ đến từ thuật toán, mà phụ thuộc rất nhiều vào chất lượng của các derived features như `packet_rate` hay SSH rolling windows. Một quy trình feature engineering vững chãi thực sự là xương sống của hệ thống phát hiện bất thường hiệu quả.
