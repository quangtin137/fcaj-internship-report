---
title: "Tuần 4 - SQS Messaging và baseline Isolation Forest"
date: 2026-05-08
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 4** — 08/05/2026 – 14/05/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần này, tôi đã tiếp cận các dịch vụ messaging của AWS và bắt tay vào mô hình hóa ML lõi. Tôi tìm hiểu cách SQS quản lý thông điệp trong pipeline real-time và tiến hành huấn luyện mô hình AI1_ISOLATION_FOREST_V1 ban đầu trên tập normal baseline.

### Mục tiêu trong tuần
* Tìm hiểu Amazon SQS và vai trò của nó trong hàng đợi thông điệp thời gian thực.
* Huấn luyện mô hình AI1 Isolation Forest baseline trên dữ liệu normal traffic.
* Hiểu rõ tham số `contamination` và khái niệm anomaly score.
* Chuẩn bị cấu trúc output ban đầu gồm `NORMAL`/`ANOMALY` kèm `anomaly_score`.

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---:|---|---|---|---|---|
| Ngày 1 | 08/05/2026 | 4 giờ | Học các khái niệm Amazon SQS và pattern tách biệt producer/consumer. | Nắm được cách SQS làm bộ đệm (buffer) cho việc nạp log realtime. | Không có vấn đề lớn. | Dùng concept SQS để lên kế hoạch cách AI1 script sẽ poll dữ liệu. | Tham dự FCAJ Community Day. |
| Ngày 2 | 09/05/2026 | 4 giờ | Tham gia FCAJ Community Day và ghi chú các nội dung về AI, định hướng học AWS và prompt engineering. | Thu thập được các bài học hữu ích cho báo cáo thực tập và reflection của project. | Cần liên hệ nội dung event với vai trò AI1 của bản thân. | Chỉ tóm tắt ý chính trong Worklog, phần chi tiết để ở Event Participated. | Áp dụng các bài học liên quan vào phần reflection của AI1. |
| Ngày 3 | 11/05/2026 | 5 giờ | Huấn luyện mô hình `AI1_ISOLATION_FOREST_V1` bằng dataset baseline 30-feature. | Mô hình học thành công biểu diễn hành vi mạng bình thường. | Tính chất không giám sát (unsupervised) khiến chưa có metric kiểm chứng ngay. | Tập trung tinh chỉnh tham số contamination để quan sát phân phối điểm số. | Phân tích anomaly scores. |
| Ngày 4 | 12/05/2026 | 5 giờ | Đánh giá phân phối output `anomaly_score` trên tập huấn luyện. | Nhận thấy cách mô hình cô lập các điểm dữ liệu. | Cần ánh xạ điểm số thô ra nhãn `NORMAL`/`ANOMALY` rõ ràng. | Chốt score direction là `higher_is_more_anomalous` cho thống nhất. | Chuẩn bị chiến lược tối ưu threshold. |
| Ngày 5 | 13/05/2026 | 4 giờ | Format cấu trúc output của AI1 để trả về `label` và `confidence`. | Hợp đồng output của AI1 đã được định nghĩa rõ cho Fusion Layer. | Không có vấn đề lớn. | Tiến hành sinh attack traffic để thực sự kiểm thử baseline này. | Chuẩn bị sinh dữ liệu tấn công. |

### Chi tiết thực hiện
Tôi đã khám phá Amazon SQS để hiểu cách nó tách biệt các nhà cung cấp dữ liệu (log shippers) khỏi hệ thống tiêu thụ (AI Engine), chuẩn bị cho việc tích hợp real-time sau này. Sau đó, tôi huấn luyện mô hình **Isolation Forest** trên normal baseline. Tôi chọn Isolation Forest vì mục tiêu của AI1 thuần túy là phát hiện bất thường (tìm sai lệch so với mức bình thường) chứ không phải là mô hình phân loại tấn công có giám sát. Mô hình xuất ra một `anomaly_score` với quy tắc `higher_is_more_anomalous`. Tôi cấu hình output để cung cấp nhãn `NORMAL` hoặc `ANOMALY` cùng với độ tin cậy (`confidence`), hoàn toàn khớp với input mà decision-level Fusion Layer mong đợi.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Do chưa có dữ liệu tấn công có nhãn, việc xác nhận hiệu năng ban đầu của Isolation Forest hoàn toàn mang tính chủ quan, chỉ dựa vào việc tự điều chỉnh tham số `contamination`.
* **Cách giải quyết:** Tôi quyết định tạm thời thiết lập tham số contamination dựa trên tỷ lệ outlier thống kê kỳ vọng trong tập baseline. Tuy nhiên, tôi thừa nhận rằng dữ liệu tấn công là bắt buộc phải có, không phải để huấn luyện, mà để đánh giá khách quan và đóng băng (freeze) ngưỡng quyết định trong những tuần tới.

### Nhận xét cá nhân
Việc huấn luyện mô hình baseline giúp logic của dự án gắn kết lại với nhau. Sự kiện FCAJ Community Day cũng củng cố thêm hiểu biết của tôi về các workflow vận hành bằng AI. Tôi học được rằng không nên ép một mô hình không giám sát vào vai trò phân loại tấn công; sức mạnh của nó nằm ở việc nhận biết hình mẫu "bình thường", khiến bước tiếp theo—kiểm thử nó với các cuộc tấn công thật—trở nên cực kỳ quan trọng.
