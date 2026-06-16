# JMeter-Performance-Testing-Lab
# BÁO CÁO KIỂM THỬ HIỆU NĂNG VỚI APACHE JMETER

* **Họ và tên:** Hoàng Như Quỳnh
* **Mã số sinh viên:** 23010027
* **Môn học:** Đánh giá và kiểm định chất lượng phần mềm
* **Công cụ sử dụng:** Apache JMeter phiên bản 5.6.3
* **Kịch bản kiểm thử chung (Thread Group):**
    * Number of Threads (users): 100 người dùng ảo
    * Ramp-up period (seconds): 10 giây
    * Loop Count: Infinite (Vô hạn) kết hợp cài đặt thời gian dừng chủ động.

## PHẦN I: KIỂM THỬ HIỆU NĂNG TRANG WEB ĐIỆN TỬ (Tuoitre.vn)

### 1. Cấu hình kịch bản (Thread Group & HTTP Request)
* **Thông số Thread Group:** 100 users, Ramp-up 10s, Duration 60s.
<img width="1917" height="1147" alt="cau hinh kich ban web" src="https://github.com/user-attachments/assets/389c4c7c-6b48-4adc-91ae-0a247f94e42e" />

* **Thông số HTTP Request:**
    * **Protocol:** https
    * **Server Name or IP:** tuoitre.vn
    * **Path:** /
    * **Content Encoding:** UTF-8
<img width="1917" height="1142" alt="cấu hình Web Tuổi Trẻ" src="https://github.com/user-attachments/assets/35272337-2177-4aae-8143-f0d88edde082" />

### 2. Kết quả kiểm thử tổng hợp (Aggregate Report)
Hệ thống tiến hành gửi tải liên tục và thu được bảng số liệu tổng hợp cố định như sau:

| Chỉ số hiệu năng | Giá trị thực tế thu được | Ý nghĩa chỉ số |
| :--- | :--- | :--- |
| **# Samples** | 1932 | Tổng số yêu cầu giả lập đã gửi lên máy chủ |
| **Average** | 3102 ms | Thời gian phản hồi trung bình của hệ thống (~3.1 giây) |
| **Median** | 2742 ms | 50% số lượng yêu cầu có thời gian phản hồi dưới 2.7 giây |
| **90% Line** | 4658 ms | 90% số lượng yêu cầu có thời gian phản hồi dưới 4.6 giây |
| **Error %** | 0.57% | Tỷ lệ lỗi cực kỳ thấp, hệ thống web vận hành siêu ổn định |
| **Throughput** | 12.7/sec | Thông lượng xử lý trung bình đạt 12.7 requests/giây |

### 3. Ảnh minh chứng kết quả Phần I
<img width="1917" height="1147" alt="web_tuoitre" src="https://github.com/user-attachments/assets/38043c47-c271-4bc6-8183-7be7e06ec64d" />

### 4. Đánh giá kết quả phần I
Trang web `tuoitre.vn` phản hồi rất tốt dưới áp lực tải từ 100 người dùng ảo truy cập cùng lúc. Tỷ lệ lỗi gần như bằng 0 (0.57%) chứng tỏ hạ tầng máy chủ Web chịu tải cực kỳ vững chắc, đáp ứng tốt nhu cầu truy cập đọc tin tức dồn dập.

## PHẦN II: KIỂM THỬ HIỆU NĂNG API DỊCH VỤ (Open-Meteo Weather API)

### 1. Cấu hình kịch bản (HTTP Request)
* **Protocol:** https
* **Server Name or IP:** api.open-meteo.com
* **Path:** /v1/forecast
* **Query Parameters (Tham số truyền vào):**
    * `latitude`: 21.0285 (Vĩ độ Hà Nội)
    * `longitude`: 105.8542 (Kinh độ Hà Nội)
    * `current_weather`: true

### 2. Kết quả kiểm thử tổng hợp (Aggregate Report)
Hệ thống tiến hành kiểm thử hiệu năng API trong vòng 10 giây và ghi nhận số liệu:

| Chỉ số hiệu năng | Giá trị thực tế thu được | Ý nghĩa chỉ số |
| :--- | :--- | :--- |
| **# Samples** | 1501 | Tổng số yêu cầu gọi API đã gửi |
| **Average** | 390 ms | Thời gian phản hồi trung bình siêu nhanh (0.39 giây) |
| **Median** | 216 ms | 50% số yêu cầu API phản hồi chỉ trong 0.21 giây |
| **90% Line** | 681 ms | 90% số yêu cầu API phản hồi dưới 0.68 giây |
| **Error %** | 39.24% | Tỷ lệ yêu cầu bị từ chối do vượt ngưỡng băng thông cho phép |
| **Throughput** | 139.6/sec | Tốc độ xử lý yêu cầu tối đa đạt 139.6 requests/giây |

### 3. Ảnh minh chứng kết quả Phần II
* **Bảng số liệu tổng hợp (Aggregate Report):**
<img width="1917" height="1145" alt="api_weather_report" src="https://github.com/user-attachments/assets/e893b3e6-bf19-4813-a7a6-c650fcb0f254" />

* **Trạng thái phản hồi chi tiết (View Results Tree):**
<img width="1917" height="1146" alt="api_weather_tree" src="https://github.com/user-attachments/assets/4cab318d-3047-449b-adf4-92596a9664fb" />

### 4. Đánh giá kết quả phần II
* **Về tốc độ phản hồi:** API dịch vụ thời tiết phản hồi cực kỳ nhanh chóng, thời gian xử lý trung vị (`Median`) chỉ tốn 216 ms, giúp trải nghiệm người dùng cuối không bị gián đoạn.
* **Về khả năng chịu tải:** Do đây là cổng API công cộng miễn phí, hệ thống có cài đặt cơ chế giới hạn tần suất gọi lệnh (Rate Limiting). Khi 100 người dùng ảo nhồi tải liên tục với thông lượng cực cao vượt mức 139 yêu cầu/giây, Server đã chủ động từ chối bớt 39.24% số yêu cầu để tự bảo vệ hệ thống không bị sập (hiển thị lỗi đỏ đan xen tích xanh trong bảng View Results Tree). Đây là cơ chế bảo mật diện rộng hoàn toàn bình thường và hợp lý của các dịch vụ API lớn hiện nay.
