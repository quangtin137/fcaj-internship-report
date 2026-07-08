---
title: "Tuần 3 - Tìm hiểu S3/RDS và thử nghiệm tiền xử lý dữ liệu ban đầu"
date: 2026-05-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
{{% notice info %}}
**Worklog Tuần 3** - 01/05/2026 - 07/05/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần này kết nối phần lưu trữ dữ liệu với các bước kiểm tra dataset đầu tiên. Tôi tìm hiểu S3 để tổ chức file thử nghiệm, xem RDS/PostgreSQL có thể lưu kết quả có cấu trúc như thế nào, rồi bắt đầu load mẫu CICIDS/CICIDS2018 bằng pandas. Bài học chính của AI1 là chất lượng tiền xử lý quan trọng hơn việc có ngay một kết quả mô hình.

### Mục tiêu trong tuần
* Hiểu cách S3 lưu raw dataset và output thử nghiệm.
* Rà soát vai trò RDS/PostgreSQL trong lưu kết quả security hoặc model có cấu trúc.
* Load mẫu CICIDS/CICIDS2018 bằng pandas và kiểm tra các cột dữ liệu.
* Kiểm tra missing value, phân phối nhãn và kiểu feature trước khi train model.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---|---|---|---|---|---|
| Ngày 1 | 01/05/2026 | 4 giờ | Tìm hiểu S3 bucket và sắp xếp lại thư mục thử nghiệm local. | Tách rõ hơn raw data, processed data và ghi chú. | Tên file ban đầu chưa thống nhất. | Dùng prefix đơn giản cho raw, processed và reports. | Rà soát lựa chọn lưu trữ có cấu trúc. |
| Ngày 2 | 02/05/2026 | 4 giờ | Ôn RDS/PostgreSQL để hiểu cách lưu record và kết quả. | Hiểu vì sao relational storage hữu ích khi cần truy vấn lịch sử detection. | Schema kết quả cuối cùng chưa rõ. | Giữ phần RDS ở mức khái niệm trong giai đoạn này. | Load mẫu dataset bằng Python. |
| Ngày 3 | 04/05/2026 | 5 giờ | Load mẫu CICIDS/CICIDS2018 bằng pandas và kiểm tra tên cột. | Xác nhận dataset có nhiều flow feature dạng số. | Một số cột có khoảng trắng và cách đặt tên dễ gây lỗi script. | Chỉ chuẩn hóa tên cột trong notebook khảo sát. | Kiểm tra missing value và label. |
| Ngày 4 | 05/05/2026 | 4 giờ | Kiểm tra missing value, kiểu feature và phân phối nhãn. | Nhận thấy dữ liệu bị lệch giữa benign và các dòng attack. | Metric thô dễ gây hiểu sai nếu bỏ qua class imbalance. | Ghi rõ vấn đề imbalance trước khi đánh giá mô hình. | Tổng hợp hạn chế tiền xử lý. |
| Ngày 5 | 06/05/2026 | 3 giờ | Viết ghi chú về hạn chế tiền xử lý và giả định của dataset. | Có cơ sở rõ hơn cho thử nghiệm model ở Tuần 4. | Không phải feature công khai nào cũng dùng lại được khi serving. | Xem cleaning code là exploratory cho đến khi hướng dữ liệu rõ hơn. | Thử Isolation Forest trên subset nhỏ. |

### Chi tiết thực hiện
S3 giúp tôi hình dung vòng đời file thử nghiệm: input thô, mẫu đã xử lý, notebook và minh chứng báo cáo. RDS/PostgreSQL được xem như nơi có thể lưu output có cấu trúc, nhưng tôi chưa giả định schema cuối.

Các bước kiểm tra bằng pandas cho thấy dataset an ninh mạng công khai cần chuẩn bị cẩn thận. Missing value, mất cân bằng nhãn và chuyển kiểu feature đều có thể ảnh hưởng cách diễn giải kết quả mô hình.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Mất cân bằng nhãn và kiểu feature lẫn lộn khiến kết quả ban đầu dễ bị hiểu sai.
* **Cách giải quyết:** Tôi ghi lại vấn đề dữ liệu trước, chưa vội kết luận về mô hình khi tiền xử lý còn chưa ổn định.

### Nhận xét cá nhân
Trước đó tôi nghĩ có dataset lớn thì bước model sẽ dễ hơn. Tuần này cho thấy điều ngược lại: dataset lớn có thể gây rối nếu chưa hiểu nhãn, kiểu feature và missing value. Phần lưu trữ AWS cũng giúp tôi nghĩ đến cách tổ chức minh chứng, không chỉ code train model. Ghi chú tiền xử lý vì vậy trở thành một phần quan trọng của báo cáo.
