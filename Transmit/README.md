"# ESP32 GGWave Audio Data Transmitter

Dự án này sử dụng ESP32 để truyền dữ liệu văn bản qua sóng âm thanh sử dụng thư viện GGWave. ESP32 nhận tin nhắn từ UART và phát chúng dưới dạng tín hiệu âm thanh có thể được giải mã bởi các thiết bị khác.

## Tính năng

- 📡 Truyền dữ liệu văn bản qua sóng âm thanh
- 🎵 Sử dụng thư viện GGWave để mã hóa âm thanh
- 💡 LED báo hiệu trạng thái truyền
- 🔘 Điều khiển bằng nút nhấn
- 📱 Giao tiếp UART để nhận tin nhắn

## Phần cứng yêu cầu

- **ESP32 Development Board**
- **LED** kết nối với chân GPIO 18
- **Loa/Buzzer** kết nối với chân GPIO 19  
- **Nút nhấn** kết nối với chân GPIO 22
- **Điện trở kéo lên** cho nút nhấn (hoặc sử dụng INPUT_PULLUP)

## Sơ đồ kết nối

```
ESP32          Component
GPIO 18   -->  LED (+ qua điện trở 220Ω)
GPIO 19   -->  Loa/Buzzer (+)
GPIO 22   -->  Nút nhấn (một đầu)
GND       -->  LED (-), Loa (-), Nút nhấn (đầu kia)
3.3V      -->  (Nếu cần nguồn cho các linh kiện)
```

## Cài đặt và Sử dụng

### Cài đặt thư viện

1. Cài đặt PlatformIO trong VS Code
2. Sao chép thư viện GGWave vào thư mục `lib/ggwave/`
3. Đảm bảo cấu trúc thư mục như sau:
   ```
   lib/
     ggwave/
       include/
         ggwave.h
       src/
         ggwave.cpp
   ```

### Cấu hình platformio.ini

```ini
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
monitor_speed = 115200
lib_deps = 
    ${PROJECT_DIR}/lib/ggwave
```

### Sử dụng

1. **Upload code** lên ESP32
2. **Mở Serial Monitor** với tốc độ 115200 baud
3. **Gửi tin nhắn** qua Serial (kết thúc bằng Enter)
4. **Nhấn nút** để phát tin nhắn qua loa
5. **LED sẽ sáng** trong quá trình truyền

## Cách thức hoạt động

### Quy trình truyền dữ liệu

1. ESP32 nhận tin nhắn văn bản từ UART
2. Tin nhắn được lưu trong bộ nhớ đệm
3. Khi nút được nhấn:
   - LED báo hiệu bật sáng
   - GGWave mã hóa văn bản thành chuỗi tần số âm thanh
   - ESP32 phát các tần số qua loa theo thứ tự
   - LED tắt khi hoàn thành

### Thông số kỹ thuật

- **Tần số lấy mẫu**: 6000 Hz
- **Samples per frame**: 128
- **Độ dài payload tối đa**: 16 bytes
- **Giao thức**: GGWAVE_PROTOCOL_MT_FASTEST
- **Format đầu vào**: 16-bit signed integer
- **Format đầu ra**: 8-bit unsigned

## Mã nguồn chính

### Cấu hình chân

```cpp
const int kPinLed0    = 18;  // LED báo hiệu
const int kPinSpeaker = 19;  // Loa/buzzer
const int kPinButton0 = 22;  // Nút nhấn
```

### Hàm truyền âm thanh

Hàm `send_text()` thực hiện:
- Mã hóa văn bản bằng GGWave
- Phát từng tần số qua loa
- Điều khiển LED báo hiệu

## Ứng dụng

- **Truyền dữ liệu không dây** trong môi trường không có WiFi/Bluetooth
- **Demo công nghệ** truyền dữ liệu qua âm thanh
- **IoT applications** với khả năng truyền dữ liệu đơn giản
- **Giáo dục** về xử lý tín hiệu số và truyền thông

## Ghi chú

- Tin nhắn tối đa 16 bytes 
- Sử dụng điện trở kéo lên nội của ESP32 cho nút nhấn
- Tần số âm thanh có thể nghe được bởi tai người
- LED báo hiệu giúp theo dõi quá trình truyền

## Troubleshooting

### Lỗi linking GGWave
Đảm bảo thư viện GGWave được đặt đúng vị trí trong `lib/ggwave/`

### Không phát được âm thanh
- Kiểm tra kết nối loa/buzzer
- Đảm bảo loa có thể phát tần số 1-8kHz

### Nút nhấn không hoạt động
- Kiểm tra kết nối nút nhấn với GPIO 22
- Đảm bảo INPUT_PULLUP được cấu hình đúng

