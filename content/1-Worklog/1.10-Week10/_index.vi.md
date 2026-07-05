---
title: "Tuần 10 - Tối ưu threshold và giải thích bằng SHAP"
date: 2026-06-19
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 10** — 19/06/2026 – 25/06/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần này, tôi tập trung vào việc tinh chỉnh hiệu năng vận hành của mô hình AI1. Tôi đã thực hiện một đợt tối ưu ngưỡng (threshold tuning) chuyên sâu nhằm cân bằng giữa False Positive Rate và Detection Rate, đồng thời khám phá công cụ SHAP để cung cấp khả năng giải thích (explainability) cho các mức điểm dị thường.

### Mục tiêu trong tuần
* Đánh giá lại `selected_threshold` (đang đóng băng ở mức 0.398066) trên các bài test tấn công đa dạng gần đây.
* Thực hiện threshold tuning để tiếp tục giảm thiểu false positive trong các edge case.
* Ứng dụng SHAP (SHapley Additive exPlanations) để giải thích xem đặc trưng nào đã dẫn đến quyết định `ANOMALY`.
* Củng cố kiến thức kiến trúc AWS liên quan đến nền tảng.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---:|---|---|---|---|---|
| Ngày 1 | 19/06/2026 | 5 giờ | Chạy script đánh giá AI1 với dải giá trị threshold quét rộng. | Vẽ được đường cong đánh đổi giữa False Positive Rate và Detection Rate. | Cần đảm bảo threshold vẫn vững vàng khi mạng có biến động nhẹ. | Tạm giữ nguyên threshold khắt khe ở mức `0.398066`; AI1 phải ưu tiên việc không gây nhiễu cảnh báo. | Tìm hiểu các công cụ giải thích model. |
| Ngày 2 | 20/06/2026 | 4 giờ | Tham gia Event Cloud Architect, cuộc thi kiến thức AWS. | Nhận ra khoảng trống kiến thức về kiến trúc AWS và có thêm động lực ôn lại các tình huống certification-style. | Một số câu hỏi kiến trúc yêu cầu kinh nghiệm triển khai thực tế nhiều hơn lý thuyết. | Đưa bài thu hoạch chi tiết vào Event Participated, trong Worklog chỉ phản ánh bài học và khoảng trống kiến thức. | Ôn lại best practices về kiến trúc AWS và liên hệ với thiết kế hệ thống. |
| Ngày 3 | 22/06/2026 | 5 giờ | Tích hợp thư viện SHAP vào script đánh giá AI1 chạy local. | Sinh được giá trị SHAP ban đầu cho một vài mẫu True Positive thuộc dạng Brute Force. | Tính toán SHAP rất tốn tài nguyên và thời gian nếu chạy realtime. | Chỉ dùng SHAP cho khâu đánh giá offline và giải thích model, không đưa vào luồng SQS streaming. | Phân tích độ quan trọng của đặc trưng. |
| Ngày 4 | 23/06/2026 | 4 giờ | Sinh các biểu đồ SHAP summary plot để giải thích trực quan quyết định của Isolation Forest. | Xác nhận các feature như `ssh_count_60s_same_src` đã tác động mạnh nhất giúp bắt Brute Force. | Không có vấn đề lớn. | Lưu tài liệu các insight SHAP này để tăng độ tin tưởng của team vào AI1. | Dùng SHAP phân tích các ca false positive. |
| Ngày 5 | 24/06/2026 | 4 giờ | Dùng SHAP để điều tra lý do một số flow bình thường bị gán nhãn `ANOMALY`. | Phát hiện model rất nhạy cảm với các outlier cực đoan của `duration` và `orig_byte_rate`. | Một số kết nối hợp lệ nhưng kéo dài quá lâu đang bị phạt điểm. | Ghi chú giới hạn này vào tài liệu kỹ thuật; Fusion Layer sẽ phải xử lý việc này thông qua tương quan log. | Chuẩn bị test end-to-end. |

### Chi tiết thực hiện
Tôi đã đào sâu vào công tác threshold tuning. Với decision rule hiện tại là `confidence >= 0.398066 => ANOMALY`, tôi quét qua các giá trị lân cận để khẳng định lại mức 0.398066 vẫn là điểm tối ưu để cân bằng False Positive Rate và Detection Rate trên tập dataset mới nhất. Nhằm "mở hộp đen" của Isolation Forest, tôi ứng dụng SHAP (SHapley Additive exPlanations) trong môi trường phân tích offline. Các biểu đồ độ quan trọng (feature importance) của SHAP tiết lộ rằng mô hình đã chấm trọng số rất cao cho các đặc trưng SSH rolling-window (vd: `ssh_count_60s_same_src`, `ssh_non_success_60s`) một cách hoàn toàn chính xác trong các pha tấn công Brute Force. Tuy nhiên, nó cũng chỉ ra rằng các đặc trưng `duration` và `packet_rate` chính là thủ phạm gây ra phần lớn các ca false positive.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Rất khó để giải thích *tại sao* AI1 lại đánh dấu một kết nối cụ thể là bất thường mỗi khi các thành viên trong team yêu cầu biện minh trong lúc phân loại các ca false positive.
* **Cách giải quyết:** Bằng cách xuất các biểu đồ SHAP force plot cho từng dòng flow cụ thể, tôi có thể chứng minh một cách khoa học chính xác đặc trưng Zeek nào đã đẩy anomaly score vượt ngưỡng. Tôi cũng làm rõ với team rằng SHAP quá chậm để chạy realtime trong pipeline SQS, nhưng lại là công cụ offline vô giá để xây dựng niềm tin vào mô hình và định hướng cho các đợt feature engineering tương lai.

### Nhận xét cá nhân
Cuộc thi Event Cloud Architect gợi nhớ cho tôi về hệ sinh thái rộng lớn của AWS, trong khi công việc hàng ngày lại yêu cầu sự tập trung cao độ vào tính minh bạch của ML. Việc sử dụng SHAP thực sự là một bước ngoặt; nó biến AI1 từ một phương trình toán học mờ mịt thành một chuỗi quyết định logic minh bạch dựa trên đặc trưng mạng. Trong lĩnh vực an ninh, một mô hình có khả năng tự diễn giải (explainable) mang lại giá trị vĩnh cửu hơn rất nhiều so với một "hộp đen" chỉ nhỉnh hơn một chút về độ chính xác.
