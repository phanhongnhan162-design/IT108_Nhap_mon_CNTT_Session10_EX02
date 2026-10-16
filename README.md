
# HƯỚNG DẪN CÀI ĐẶT CẢM BIẾN ĐỘ ẨM CHO TRANG TRẠI LAN.

## 1. Tổng quan
- Cảm biến độ ẩm giúp theo dõi độ ẩm không khí trong nhà lưới, đảm bảo môi trường tối ưu cho Lan phát triển

**Thiết bị cần chuẩn bị (task list):**
- ✅ Cảm biến độ ẩm (DHT22 / AM2302)
- ✅ Raspberry Pi / Jetson / soC
- ✅ Dây nối Dupont
- ✅ Tua vít, băng keo, ốc vít
- ✅ Thẻ nhớ đã cài hệ điều hành

**Bảng so sánh cảm biến cũ và mới**

| Tiêu chí | Cảm biến DHT11 (Cũ) | Cảm biến DHT22 (Mới) |
| :--- | :--- | :--- |
| Dải đo độ ẩm| 20% - 90% | 0% - 100% |
| Độ chính xác | Thấp (±5%) | Cao (±2%) |
| Giá thành | Rẻ hơn | Đắt hơn |
| Ứng dụng| Học tập cơ bản | Nhà kính, trang trại lan |

## 2. Các bước cài đặt:
**1. Cập nhật hệ thống:**
- Mở Terminal và cập nhật danh sách gói.
```bash
sudo apt update && sudo apt upgrade -y
```
 **2. Cài đặt thư viện cho cảm biến DHT22**
 - Cài đặt thư viện đọc dữ liệu từ cảm biến.
```bash
sudo apt install -y git python3-pip
sudo pip3 install Adafruit_DHT
 ```
**3. Kích hoạt giao tiếp 1-Wire (nếu dùng DHT22)** 
- Bật giao tiếp 1-Wire trên hệ thống.
```bash
sudo raspi-config                # Chọn: interface Options >
                                 # 1-Wire > Enable
``` 
**4. Kiểm tra module đã được nhận**
- Sau khi bật 1-Wire, kiểm tra thiết bị.
```bash
ls /sys/bus/w1/devices/          # Kết quả có dạng 28-xxxxxxx
```
**5. Chạy thử chương trình đọc cảm biến**
- Tải và chạy file Python mẫu.
```bash
git clone https://github.com/adafuit/Adafruit_Python_DHT.git
cd Adafruit_Python_DHT/examples
```
```bash
python3 AdafruitDHT.py 22 4      # 22: DHT22, 4: GPIO pin 
                                 # Nhận kết quả độ ẩm và nhiệt độ
```
**6. Tự động chạy khi khởi động (tùy chọn)**
- Thêm vào contab để đọc dữ liệu định kỳ.
```bash
crontab -e                       # Thêm dòng(ví dụ mỗi 5 phút)                            
                                 */5 * * * * python3 /home/pi/read_dht22.py
```
### 3. Sơ đổ kết nối
| DHT22 | Raspberry Pi (BCM) |
|:---|:---|
|🟥 VCC | 3.3V (Pin 1)|
|⬛ GND | GND (Pin 6)|
|🟨 Data| GPIO4 (Pin 7)|
|⬜ NC| (không kết nối)|

#### 4. Vị trí lắp đặt
- Lắp cảm biến ở nơi thoáng, tránh ánh nắng trực tiếp và  nước bắn vào.

##### 5. Kiểm tra kết quả.
```bash
Temp=28.4*    Humidity=67.1%
pi@farm:~ $
```

