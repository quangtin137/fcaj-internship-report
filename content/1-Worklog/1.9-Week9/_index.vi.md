---
title: "Tuần 9 - Kiểm tra dataset mismatch và khả năng tổng quát hóa"
date: 2026-06-12
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 9** — 12/06/2026 – 18/06/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần này, tôi tạm lùi lại khỏi việc lập trình pipeline để tiến hành một đợt rà soát quan trọng về hiện tượng lệch dữ liệu (dataset mismatch). Tôi đã thử nghiệm việc đưa các tập dữ liệu public (như Kaggle) vào bổ sung cho dữ liệu Zeek lab chính, nhằm cải thiện khả năng tổng quát hóa (generalization) của mô hình Isolation Forest AI1.

### Mục tiêu trong tuần
* Đánh giá lý thuyết về hiện tượng train-serving dataset mismatch.
* Khảo sát các public/Kaggle dataset xem có thể bổ sung cho `conn_dataset` nội bộ hay không.
* Chuẩn hóa schema của public dataset về đúng chuẩn 30-feature schema của Zeek.
* Đánh giá xem việc gộp dữ liệu ngoài có thực sự giúp AI1 tổng quát hóa tốt hơn không.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---:|---|---|---|---|---|
| Ngày 1 | 12/06/2026 | 5 giờ | Đọc tài liệu về PSI (Population Stability Index) và độ lệch dữ liệu trong các model network anomaly. | Hiểu rằng model chỉ train trên một topology mạng duy nhất rất dễ fail khi ra thực tế. | Hiện tại AI1 chỉ phụ thuộc vào `conn_dataset` của lab. | Tìm kiếm public dataset bổ sung để đa dạng hóa normal baseline và vector tấn công. | Tìm data tương thích trên Kaggle. |
| Ngày 2 | 13/06/2026 | 5 giờ | Tìm được một số public dataset về intrusion detection và so sánh phân phối feature với schema Zeek. | Hầu hết public dataset dùng công cụ trích xuất flow khác (không phải Zeek). | Lệch schema nghiêm trọng; nhiều public feature không thể map sang chuẩn 30-feature của lab. | Loại bỏ ngay những dataset không thể map chuẩn xác về định dạng Zeek `conn.log`. | Viết script chuyển đổi schema. |
| Ngày 3 | 15/06/2026 | 6 giờ | Viết script để map các trường hợp lệ của public data sang schema Zeek, điền `-` nếu cần. | Ghép thành công một phần public data vào tập huấn luyện. | Việc ghép data ngây thơ khiến Isolation Forest đánh dấu luôn các dữ liệu "normal" mới là anomaly. | Baseline "normal" của public dataset có các đặc trưng tỷ lệ (vd: `bytes_per_pkt`) hoàn toàn khác biệt. | Cân nhắc hướng domain classification. |
| Ngày 4 | 16/06/2026 | 5 giờ | Đánh giá mô hình AI1 trên tập dữ liệu đã gộp. | Nhận thấy việc tổng quát hóa xuyên suốt các domain mạng khác nhau là cực kỳ khó. | Trộn data bừa bãi làm chỉ số False Positive Rate trên tập lab data chính bị suy giảm. | Quay về dùng `conn_dataset` của lab làm dữ liệu chính. Public data chỉ làm dữ liệu phụ trợ để test các edge case. | Ghi chú lại kết luận về mismatch. |

### Chi tiết thực hiện
Tôi đã khảo sát tiềm năng của các public dataset trong việc nâng cấp AI1. Tôi nhận thức rõ schema 30-feature của AI1 bị trói buộc chặt chẽ với logic trích xuất `conn.log` của Zeek. Rất nhiều public dataset cũng cung cấp flow-level feature, nhưng định nghĩa của họ về `duration` hay `packet_rate` thường có sai khác nhỏ so với Zeek. Sau khi viết script chuyển đổi một public dataset về đúng schema của team, tôi phát hiện ra một sự lệch dữ liệu nghiêm trọng. Mô hình Isolation Forest vốn đã học rất chuẩn "normal" baseline theo topology lab của chúng tôi, nay lập tức đánh dấu mọi lưu lượng "normal" của public dataset là các bất thường điểm cao. Thử nghiệm thực tế này đã chứng minh tính đúng đắn của chiến lược dự án ban đầu: **Zeek-first dataset strategy**, trong đó dữ liệu tự thu tại lab là cốt lõi, thay vì nhắm mắt tin dùng các public dataset đại trà.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Khi cố gắng gộp public data, các phân phối đặc trưng (đặc biệt là nhóm rate/ratio như `orig_byte_rate` và `orig_resp_byte_ratio`) khác biệt đến mức cơ bản, khiến tỷ lệ False Positive Rate của mô hình tăng phi mã.
* **Cách giải quyết:** Tôi ghi chép lại hiện tượng này dưới góc độ của bài toán domain adaptation. Tôi đi đến kết luận rằng với AI1, bắt buộc phải dựa vào `conn_dataset` nội bộ để huấn luyện normal baseline. Dữ liệu public chỉ nên dùng làm tập test đánh giá tấn công phụ trợ, với điều kiện nó phải được chuyển đổi an toàn sang schema Zeek mà không bị biến dạng (distortion).

### Nhận xét cá nhân
Tuần này mang lại một bài học data science thực chiến sâu sắc. Người ta rất dễ mặc định "càng nhiều data càng tốt", nhưng trong an ninh mạng, dữ liệu gắn liền mật thiết với topology sinh ra nó. Thử nghiệm dataset mismatch đã củng cố vững chắc nhận thức của tôi: việc feature engineering đồng nhất (`feature extractor` consistency) quan trọng hơn gấp nhiều lần so với việc cứ ném những tập Kaggle khổng lồ vào thuật toán.
