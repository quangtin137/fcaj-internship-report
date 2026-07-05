---
title: "Tuần 5 - Thu attack traffic và chiến lược gán nhãn"
date: 2026-05-15
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
{{% notice info %}}
📋 **Worklog Tuần 5** — 15/05/2026 – 21/05/2026
{{% /notice %}}

### Tổng quan trong tuần
Tuần này dành riêng cho việc sinh và gán nhãn dữ liệu tấn công. Vì AI1 là một mô hình phát hiện bất thường không giám sát, dữ liệu tấn công này không dùng để huấn luyện mà chỉ dùng để đánh giá mô hình và đóng băng (freeze) ngưỡng quyết định.

### Mục tiêu trong tuần
* Mô phỏng các cuộc tấn công mạng chủ động (Port Scan, Brute Force) trong lab.
* Lập nhật ký tấn công (attack diary) chặt chẽ để theo dõi mốc thời gian.
* Xây dựng chiến lược gán nhãn dựa trên cửa sổ thời gian (timestamp window) và session UID.
* Dùng dữ liệu đã gán nhãn để đánh giá AI1 mà không biến nó thành mô hình phân loại tấn công (supervised classifier).

### Nhật ký hằng ngày
| Ngày | Ngày thực tế | Thời gian | Công việc đã hoàn thành | Kết quả | Vấn đề | Quyết định | Bước tiếp theo |
|---|---|---:|---|---|---|---|---|
| Ngày 1 | 15/05/2026 | 5 giờ | Chạy các lệnh Nmap Port Scan và Hydra Brute Force tấn công server lab. | Sinh ra lưu lượng tấn công thực tế xen kẽ với lưu lượng bình thường. | Dữ liệu chỉ toàn tấn công thì không sát thực tế. | Thực hiện tấn công trong lúc vẫn chạy script sinh HTTP/DNS traffic bình thường. | Lập attack diary. |
| Ngày 2 | 16/05/2026 | 4 giờ | Tổng hợp nhật ký tấn công ghi rõ mốc thời gian bắt đầu/kết thúc của từng đợt. | Có bản ghi chính xác về thời điểm xảy ra bất thường trên mạng. | Có độ lệch đồng hồ nhẹ giữa máy tấn công và server. | Dùng cửa sổ thời gian ±2 giây khi gán nhãn để bù trừ sai số. | Viết script gán nhãn. |
| Ngày 3 | 18/05/2026 | 5 giờ | Viết script Python để merge attack diary với Zeek `conn.log` phục vụ gán nhãn. | Dataset đã có nhãn rõ ràng để dùng cho khâu đánh giá. | Một số kết nối bình thường kéo dài bị trùng vào cửa sổ tấn công. | Tinh chỉnh logic gán nhãn, kiểm tra thêm Source IP và Port đích ngoài timestamp. | Đánh giá AI1 với dữ liệu có nhãn. |
| Ngày 4 | 19/05/2026 | 6 giờ | Đánh giá Isolation Forest AI1 trên dataset vừa được gán nhãn. | Quan sát được phản ứng của `anomaly_score` với tấn công thật so với lúc mạng bình thường. | AI1 không chỉ ra loại tấn công gì, chỉ biết là có bất thường. | Tuân thủ đúng thiết kế: phân loại tấn công cụ thể là việc của AI2A, không phải AI1. | Chuẩn bị đóng băng các artifact. |

### Chi tiết thực hiện
Tôi đã sinh các cuộc tấn công tổng hợp bằng Nmap và Hydra. Để mô phỏng môi trường production thực tế, các cuộc tấn công này được bắn vào hệ thống trong lúc lưu lượng baseline bình thường vẫn đang chạy liên tục. Tôi ghi chép cẩn thận nhật ký tấn công và viết script gán nhãn dữ liệu `conn.log` dựa trên các cửa sổ thời gian, IP nguồn và UID của phiên kết nối. Sau khi gán nhãn, tôi chạy lại mô hình baseline Isolation Forest (vốn chỉ học từ dữ liệu normal) trên tập dữ liệu mới này. Tôi đặc biệt chú ý đến cách các đặc trưng phái sinh như `ssh_non_success_60s` phản ứng lại trong các đợt Brute Force, qua đó quan sát thấy sự tăng vọt tương ứng của `anomaly_score`.

### Khó khăn & Cách xử lý
* **Vấn đề gặp phải:** Rất dễ bị cám dỗ dùng các nhãn tấn công vừa tạo để huấn luyện một mô hình phân loại có giám sát (supervised classifier), làm lu mờ ranh giới giữa AI1 và AI2A.
* **Cách giải quyết:** Tôi tuân thủ nghiêm ngặt đề cương kiến trúc. AI1 phải giữ nguyên bản chất là mô hình không giám sát (unsupervised anomaly detector). Dữ liệu có nhãn chỉ được dùng độc quyền cho việc đo lường False Positive Rate và Detection Rate, đảm bảo AI1 chỉ xuất ra `NORMAL` hoặc `ANOMALY` + `confidence`. Điều này giữ cho sự phân chia nhiệm vụ được rành mạch trước khi đưa vào decision-level fusion.

### Nhận xét cá nhân
Tuần này làm rõ sự khác biệt giữa phát hiện bất thường và phân loại mối đe dọa. Nhìn thấy `anomaly_score` tăng vọt chính xác vào thời điểm tôi chạy Nmap đem lại cảm giác rất thỏa mãn. Nó chứng minh rằng chiến lược Zeek-first dataset và quá trình feature engineering kỹ lưỡng của chúng tôi đã phát huy tác dụng.
