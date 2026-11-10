# SƠ ĐỒ ĐẤU NỐI XE DÒ LINE TỰ ĐỘNG (ARDUINO NANO \& ĐỘNG CƠ GA25)

Tài liệu hướng dẫn đấu nối chi tiết hệ thống phần cứng xe dò line tự động sử dụng **Arduino Nano**, **Driver TB6612FNG**, **2 Động cơ GA25 (Encoder 5 chân)**, **Cảm biến dò line 8 mắt** và **LiDAR VL53L0X**.

\---

## 1\. HỆ THỐNG NGUỒN \& HẠ ÁP

> ⚠️ \*\*CẢNH BÁO NGUY HIỂM:\*\* 
> Bắt buộc dùng đồng hồ vạn năng đo và vặn biến trở trên Mạch Hạ Áp về đúng \*\*5.0V\*\* TRƯỚC KHI cắm vào Arduino và các cảm biến. Nguồn 12V trực tiếp từ Pin LiPo sẽ làm cháy hỏng vi điều khiển lập tức.

|Linh kiện nguồn / Chân|Nối tới đâu?|Chức năng / Ghi chú|
|-|-|-|
|**Pin LiPo (12V (+))**|`IN+` Mạch Hạ Áp **\&** `VM` TB6612FNG|Cấp nguồn 12V cho động cơ và hạ áp|
|**Pin LiPo (12V (-))**|`IN-` Mạch Hạ Áp **\&** GND Chung|Mass hệ thống|
|**Mạch Hạ Áp (`OUT+`)**|Chân **5V** Arduino, VCC Cảm biến \& TB6612|Nguồn 5V ổn định nuôi toàn bộ vi mạch|
|**Mạch Hạ Áp (`OUT-`)**|**GND Chung**|Nối Mass toàn bộ mạch điện với nhau|

\---

## 2\. DRIVER ĐỘNG CƠ (TB6612FNG)

|Chân trên TB6612FNG|Kết nối tới (Arduino / Nguồn)|Ghi chú lập trình / Chức năng|
|-|-|-|
|**VM / VM1 / VM2 / VM3**|Pin LiPo 12V (+)|Nguồn kéo Động cơ GA25 (Nối chập 3 chân VM)|
|**VCC**|5V (Mạch hạ áp)|Nguồn nuôi chip logic TB6612FNG|
|**GND**|GND Chung|Mass|
|**STBY**|5V (Mạch hạ áp)|Kích hoạt mạch (Mức HIGH)|
|**PWMA**|Chân **D5** (Arduino Nano)|Băm xung PWM điều khiển tốc độ Động cơ Trái|
|**AIN1**|Chân **D6** (Arduino Nano)|Điều khiển chiều quay Động cơ Trái|
|**AIN2**|Chân **D7** (Arduino Nano)|Điều khiển chiều quay Động cơ Trái|
|**PWMB**|Chân **D10** (Arduino Nano)|Băm xung PWM điều khiển tốc độ Động cơ Phải|
|**BIN1**|Chân **D8** (Arduino Nano)|Điều khiển chiều quay Động cơ Phải|
|**BIN2**|Chân **D9** (Arduino Nano)|Điều khiển chiều quay Động cơ Phải|
|**AO1 \& AO2**|2 chân `M+` và `M-` của **GA25 Trái**|Cấp nguồn xoay cho động cơ trái|
|**BO1 \& BO2**|2 chân `M+` và `M-` của **GA25 Phải**|Cấp nguồn xoay cho động cơ phải|

\---

## 3\. CỤM 2 ĐỘNG CƠ GA25 + ENCODER (5 CHÂN)

Loại encoder 5 chân xuất xung 1 kênh tín hiệu (OUT/SIG).

### 3.1 Động cơ GA25 bên Trái

|Chân đuôi Động cơ|Kết nối tới|Chức năng|
|-|-|-|
|**M+ / M1**|Chân **AO1** (TB6612FNG)|Cực động cơ (Đảo dây nếu quay ngược)|
|**M- / M2**|Chân **AO2** (TB6612FNG)|Cực động cơ|
|**VCC / +**|5V (Mạch hạ áp)|Nuôi mắt đọc Encoder|
|**GND / -**|GND Chung|Mass|
|**OUT / S / SIG**|Chân **D2** (Arduino Nano)|**Bắt buộc chân D2 (Ngắt ngoài 0)** để đọc xung|

### 3.2 Động cơ GA25 bên Phải

|Chân đuôi Động cơ|Kết nối tới|Chức năng|
|-|-|-|
|**M+ / M1**|Chân **BO1** (TB6612FNG)|Cực động cơ (Đảo dây nếu quay ngược)|
|**M- / M2**|Chân **BO2** (TB6612FNG)|Cực động cơ|
|**VCC / +**|5V (Mạch hạ áp)|Nuôi mắt đọc Encoder|
|**GND / -**|GND Chung|Mass|
|**OUT / S / SIG**|Chân **D3** (Arduino Nano)|**Bắt buộc chân D3 (Ngắt ngoài 1)** để đọc xung|

\---

## 4\. CẢM BIẾN DÒ LINE (8 MẮT QUANG TRỞ)

|Mắt đọc|Chân Arduino Nano|Cấu hình trong Code|
|-|-|-|
|**VCC**|5V (Mạch hạ áp)|Nuôi mạch cảm biến|
|**GND**|GND Chung|Mass|
|**Mắt 1** (Trái ngoài cùng)|Chân **D12**|`digitalRead(12)`|
|**Mắt 2**|Chân **D13**|`digitalRead(13)`|
|**Mắt 3**|Chân **A0**|`digitalRead(A0)`|
|**Mắt 4**|Chân **A1**|`digitalRead(A1)`|
|**Mắt 5**|Chân **A2**|`digitalRead(A2)`|
|**Mắt 6**|Chân **A3**|`digitalRead(A3)`|
|**Mắt 7**|Chân **A6**|`analogRead(A6) > 500` *(Xem lưu ý phía dưới)*|
|**Mắt 8** (Phải ngoài cùng)|Chân **A7**|`analogRead(A7) > 500` *(Xem lưu ý phía dưới)*|

> 📌 \*\*LƯU Ý QUAN TRỌNG VỀ CHÂN A6 \& A7:\*\*
> Trên vi điều khiển ATmega328P của Arduino Nano, 2 chân \*\*A6\*\* và \*\*A7\*\* là cổng Analog thuần túy, \*\*KHÔNG hỗ trợ lệnh `digitalRead()`\*\*. 
> Khi lập trình cho Mắt 7 và Mắt 8, bạn phải dùng lệnh `analogRead(A6)` và so sánh với một mức ngưỡng (ví dụ: `if (analogRead(A6) > 500)`) để nhận biết màu trắng/đen.

\---

## 5\. CẢM BIẾN KHOẢNG CÁCH LiDAR (VL53L0X)

Cảm biến giao tiếp qua chuẩn bus I2C.

|Chân VL53L0X|Chân Arduino Nano|Ghi chú|
|-|-|-|
|**VIN / VCC**|5V (Mạch hạ áp)|Nguồn nuôi module|
|**GND**|GND Chung|Mass|
|**SCL**|Chân **A5**|Xung nhịp I2C phần cứng|
|**SDA**|Chân **A4**|Dữ liệu I2C phần cứng|

\---

## 6\. LƯU Ý KHI VẬN HÀNH GA25 VỚI TB6612FNG

1. **Công suất \& Bảo vệ mạch:** Động cơ GA25 có lực kéo lớn. Khi xe chạy bình thường dòng chỉ khoảng `200mA - 300mA`. Tuy nhiên, nếu xe bị đâm vào chướng ngại vật hoặc **kẹt bánh**, dòng điện có thể vọt lên trên `1.5A`. Nếu bị kẹt bánh, hãy **ngắt nguồn ngay lập tức** để tránh quá nhiệt làm hỏng TB6612FNG.
2. **Đảo chiều động cơ:** Nếu kích hoạt xe chạy tiến mà một trong hai bánh lại quay lùi, bạn chỉ cần nạp lại code hoặc hoán đổi vị trí 2 dây `M+` và `M-` của động cơ đó ngay tại cọc đấu dây của TB6612FNG.

