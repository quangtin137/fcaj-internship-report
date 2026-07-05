---
title: "Tuần 12 - Demo cuối kỳ, hoàn thiện báo cáo và tổng kết thực tập"
date: 2026-07-03
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 12** — 03/07/2026 – 10/07/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần cuối cùng này là đỉnh cao của toàn bộ 12 tuần thực tập. Nhóm đã thực hiện buổi demo live hệ thống Hybrid IDS, chứng minh khả năng hoạt động thời gian thực (real-time). Tôi cũng hoàn thiện báo cáo thực tập kỹ thuật, tổng kết lại toàn bộ đóng góp của mình cho module AI1.

### Mục tiêu trong tuần
* Thực hiện demo hệ thống cuối kỳ với các kịch bản tấn công live Port Scan và Brute Force.
* Đảm bảo AI1 nhận đúng đủ 30 feature, xử lý mượt mà và đẩy output sang Fusion Layer theo thời gian thực.
* Hoàn thiện báo cáo thực tập tốt nghiệp.
* Tổng kết lại hành trình từ lúc học cơ bản AWS đến lúc triển khai thành công một pipeline ML bằng container.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---:|---|---|---|---|---|
| Ngày 1 | 03/07/2026 | 5 giờ | Chạy diễn tập (dry run) cho demo, dọn sạch (purge) hàng đợi SQS để đảm bảo trạng thái sạch. | Pipeline đã được xác thực và sẵn sàng cho live demo. | Gặp phải chút độ trễ cold-start của ECS trong lần chạy thử đầu tiên. | Giữ các task ECS của AI Engine ở trạng thái active liên tục ngay trước giờ demo để tránh latency. | Chạy demo chính thức. |
| Ngày 2 | 04/07/2026 | 5 giờ | Cùng team thực hiện live demo, trực tiếp bắn các lệnh tấn công Nmap và Hydra. | Phát hiện realtime thành công. `label`, `confidence`, và `selected_threshold` của AI1 đổ về pipeline hoàn hảo. | Thỉnh thoảng Dashboard cần phải refresh bằng tay. | Giải thích cho người xem hiểu rằng luồng tổng hợp backend diễn ra gần như tức thời, dù frontend cần refresh. | Viết báo cáo tổng kết. |
| Ngày 3 | 06/07/2026 | 6 giờ | Chắp bút những phần cuối của báo cáo thực tập AI1 Network Anomaly Detection. | Trình bày lại toàn bộ phương pháp luận, chiến lược dataset, feature engineering và tích hợp inference. | Cần cẩn thận không vơ nhận công trạng của phần AI2A hay Suricata vào báo cáo cá nhân. | Duy trì ranh giới phạm vi nghiêm ngặt của AI1 trong các văn bản nộp chính thức. | Review format báo cáo. |
| Ngày 4 | 07/07/2026 | 4 giờ | Rà soát định dạng báo cáo, sửa lỗi chính tả, đảm bảo các thuật ngữ kỹ thuật (như `conn_dataset`, `fail_if_missing`) dùng đúng chỗ. | Bản tài liệu chuyên nghiệp đã sẵn sàng để nộp. | Không có vấn đề lớn. | Tiến hành nộp chính thức. | Nộp báo cáo thực tập. |
| Ngày 5 | 09/07/2026 | 3 giờ | Chính thức nộp báo cáo và tiến hành buổi retrospective cuối cùng cùng cả team. | Kỳ thực tập chính thức khép lại. | Không có vấn đề lớn. | Ăn mừng kỳ thực tập kết thúc thành công rực rỡ. | Kết thúc kỳ thực tập. |

### Chi tiết thực hiện
Nhiệm vụ kỹ thuật chính là đảm bảo sự ổn định tuyệt đối cho buổi live demo. Tôi kiểm tra chắc chắn hàng đợi SQS đã được dọn dẹp tin nhắn cũ để ngăn dữ liệu tồn đọng gây ra cảnh báo sai. Trong quá trình chạy kịch bản Port Scan và Brute Force, tôi theo dõi sát log của AI1 container. Tôi kiểm tra trực tiếp (explicitly check) các payload truyền vào chứa đủ 30 feature, giúp AI1 vượt qua chốt chặn `fail_if_missing` thành công. Tôi quan sát thấy `anomaly_score` vọt qua ngưỡng `0.398066` đã đóng băng, xuất ra nhãn `ANOMALY` mượt mà, tích hợp thẳng vào Fusion Layer để kích hoạt cảnh báo Dashboard cuối cùng. Sau buổi demo, tôi hoàn thiện báo cáo, đảm bảo nó phản ánh chính xác chiến lược Zeek-first dataset, quá trình nhào nặn đặc trưng cho Isolation Forest và các hợp đồng schema cực kỳ nghiêm ngặt mà tôi đã thiết lập.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Trong lúc dry run, việc giữ cho toàn bộ pipeline đồng bộ khá là căng thẳng; nếu Feature Router vô tình rớt mất một trường dữ liệu, AI1 sẽ từ chối toàn bộ tin nhắn đó, có nguy cơ phá hỏng buổi live demo.
* **Cách giải quyết:** Chính sự nhất quán schema vô cùng khắt khe mà chúng tôi thiết lập từ Tuần 6 và 7 đã cứu vãn tình hình. Nhờ có hợp đồng nghiêm ngặt, bất kỳ lỗi nào cũng phơi bày ngay lập tức lúc chạy nháp, cho phép chúng tôi vá lỗi router thượng nguồn cực kỳ nhanh chóng. Tới lúc live demo, pipeline đã vững như bàn thạch (rock-solid).

### Nhận xét cá nhân
Hoàn thành chặng đường 12 tuần mang lại cho tôi một cảm giác cực kỳ tự hào. Từ những bài học AWS vỡ lòng, tôi đã làm chủ một thành phần AI cốt lõi trong một pipeline bảo mật cloud-native phức tạp. Cảm giác theo vết một gói tin mạng từ lúc là mã độc tấn công, chạy qua Zeek, biến thành vector 30-feature, được đánh giá bởi Isolation Forest, và cuối cùng hiện lên thành cảnh báo đỏ rực trên Dashboard là một trải nghiệm không thể nào quên. Kỳ thực tập này đã củng cố vững chắc ngọn lửa đam mê của tôi tại điểm giao thoa giữa Cloud Computing, Data Engineering và Trí tuệ Nhân tạo.
