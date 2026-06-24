# 🦾 Bruce ESP32-C5 — AnhNhat07 Edition

Firmware Bruce tùy chỉnh cho **ESP32-C5** với màn hình **ST7789 1.47" 172×320**.

---

## ✨ Tính năng đã mod

- 🎯 **Deauth nâng cao**: Channel hopping thông minh, reason code rotation
- 📡 **SSID Grouping**: Gom mesh/dual-band theo SSID, hiển thị tất cả BSSID
- 💪 **TX Power tối đa**: 84 units (~20 dBm) + disassoc frame
- 🖥️ **Status bar**: Hiển thị `AnhNhat07 | HH:MM:SS`
- 🖼️ **Boot screen**: Hỗ trợ `boot.jpg` và `boot.png` từ SD card
- 📊 **Scan header**: Hiển thị số BSSID / SSID đã scan
- ℹ️ **Info screen**: Hiển thị toàn bộ BSSID trong group (mesh aware)
- 🤝 **Handshake Capture nâng cao**: mesh/dual-band (kênh từ scan), đổi kênh LEFT/RIGHT, UI cuộn đầy đủ, PCAP từng BSSID
- 🛡️ **CSA PMF Bypass**: Bypass 802.11w PMF trên Android/Samsung bằng fake beacon với Channel Switch Announcement IE
- 🔄 **Rescan**: Scan lại danh sách WiFi ngay trong menu không cần thoát
- 🔓 **WPA3 Downgrade Attack**: Clone AP với captive portal tiếng Việt đẹp, deauth liên tục 60s
- 🎯 **WPA3 Transition Mode Exploit**: CSA beacon injection + handshake capture tự động, bypass PMF

---

## 📋 Changelog

### v1.0.7
- **Multi Deauther**: Tích hợp CSA (Channel Switch Announcement - tag 37) Beacons để bypass PMF/802.11w trên cả băng tần 2.4GHz và 5GHz
- **Multi Deauther**: Đồng bộ sức mạng với Target Deauther, giảm độ trễ chuyển kênh từ 50ms xuống còn 5ms (nhanh gấp 10 lần)
- **Multi Deauther**: Tích hợp bộ xoay vòng qua 6 mã lý do ngắt kết nối khác nhau (Reason Code Rotation)
- **Multi Deauther**: Bắn gói tin cường độ cao (15 CSA beacons, 20 deauths và 20 disassocs ở tốc độ tối đa của phần cứng)
- **Sửa lỗi**: Khắc phục triệt để lỗi logic khởi tạo APSTA trong hàm `wifi_atk_setWifi()`, sửa lỗi deauth thất bại sau khi quét mạng
- **Hiệu năng**: Ép công suất phát sóng (TX Power) tối đa `84` (~21dBm), tối ưu hóa WDT tránh sập/treo máy khi chạy lâu dài

### v1.0.6
- **Handshake Capture**: Truyền `ssidGroup` — chỉ dùng kênh thật từ scan (mesh/dual-band), không quét 1–14
- **Handshake Capture**: LEFT cuộn lên / lùi kênh (ở đầu danh sách); RIGHT cuộn xuống hoặc kênh tiếp theo (cuối danh sách)
- **Handshake Capture**: Chọn BSSID RSSI mạnh nhất trên mỗi kênh; đăng ký PCAP cho mọi BSSID trong nhóm
- **Handshake Capture**: UI panel căn giữa, mục đầy đủ (network, target, kênh, BSSIDs, EAPOL M1–M4, activity), cuộn nội dung
- **Handshake Capture**: Footer gọn 2 dòng (`L:up R:dn/ch SEL:deauth`, `BACK: stop`) — tối ưu màn 172×320, 3 nút

### v1.0.5
- **WPA3 Downgrade Attack**: Clone AP target với captive portal tiếng Việt (gradient design, animations, countdown timer)
- **WPA3 Downgrade**: Deauth liên tục 60 giây, tự động dừng khi bắt được password
- **WPA3 Downgrade**: Lưu credentials vào `vietnamese_creds.csv`
- **WPA3 Transition Mode Exploit**: CSA beacon injection (tag 37) + Disassoc flood để bypass PMF
- **WPA3 Transition Mode**: Tự động bắt WPA2 handshake khi client downgrade, hiển thị EAPOL counter (0-4)
- **WPA3 Transition Mode**: Lưu handshake vào `/BrucePCAP/handshakes/HS_[BSSID]_[SSID].pcap`
- **WPA3 Transition Mode**: Hỗ trợ mesh/dual-band (round-robin attack tất cả BSSID trong group)
- **Evil Portal**: Background mode — portal chạy ngầm trong khi deauth tiếp tục
- **Evil Portal**: Auto template selection — Vietnamese template tự động load cho WPA3 downgrade
- **Menu**: Thêm "WPA3 Downgrade" vào WiFi → Wifi Atks
- **Menu**: Thêm "WPA3 CSA+Disassoc" vào target menu (khi chọn 1 AP cụ thể)

### v1.0.4
- **Deauth**: Thêm `send_csa_burst()` — inject beacon giả với CSA IE (tag 37) để bypass PMF/802.11w trên Android/Samsung. Beacon frame không được bảo vệ bởi PMF → buộc client disconnect
- **Deauth**: Gửi 10 CSA beacon sau mỗi deauth burst trong `target_atk`
- **Scan**: Thêm `[↺ Rescan]` ở cuối menu SSID list — scan lại không cần thoát menu

### v1.0.3
- **Handshake Capture**: Chuyển sang `WIFI_MODE_APSTA` + `softAP` để `send_raw_frame` hoạt động đúng (fix deauth không gửi được trong v1.0.2)
- **Handshake Capture**: Thêm `send_probe_burst` mỗi 5s (5–10 frames) để kích thích client reconnect
- **Handshake Capture**: `lastAutoDeauth = millis()` — chờ đủ 10s trước deauth đầu tiên, tránh deauth khi sniffer chưa khởi động xong
- **Handshake Capture**: Hiển thị `Probes(all ch)` thay vì `Probes Rx` — rõ ràng đây là tổng probe trên toàn channel
- **Handshake Capture**: Hiển thị EAPOL M1/M2/M3/M4 realtime, stats Probes Tx / Deauth count
- **Sniffer**: Thêm `num_probes` counter đếm Probe Request frames
- **Sniffer**: Log EAPOL frame mới ra Serial để debug

### v1.0.2
- **Handshake Capture**: Auto-deauth loop mỗi 10s với reason code rotation (6 codes)
- **Handshake Capture**: Thêm disassoc frame `0xA0` song song với deauth `0xC0`
- **Deauth**: Tăng `FRAMES_PER_AP` lên 20 (deauth + disassoc)
- **Deauth**: Reason code rotation mỗi 30 frames

### v1.0.1
- **Deauth**: Channel hopping thông minh, verify channel mỗi 15s
- **SSID Grouping**: Gom mesh/dual-band theo SSID
- **TX Power**: Set max 84 units (~20 dBm)
- **Status bar**: `AnhNhat07 | HH:MM:SS`
- **Boot screen**: Hỗ trợ `boot.jpg` / `boot.png` từ SD card

### v1.0.0
- Initial release — port Bruce lên ESP32-C5 + ST7789 1.47"

---

## 🔧 Hardware

| Thành phần | Model |
|-----------|-------|
| MCU | Ebyte E101-C5WN8-XS (ESP32-C5) |
| Display | ST7789 1.47" 172×320 (Landscape) |
| Antenna | 8dBi external via IPEX |
| Flash | 8MB |
| PSRAM | 8MB |

---

## 📡 Hướng Dẫn Kết Nối

### SPI Bus (Shared — nối tới TẤT CẢ module SPI)

```
GPIO 7  (MOSI) ──┬── TFT MOSI
                 ├── SD  MOSI
                 ├── CC1101 MOSI
                 └── NRF24  MOSI

GPIO 2  (MISO) ──┬── TFT MISO
                 ├── SD  MISO
                 ├── CC1101 MISO
                 └── NRF24  MISO

GPIO 6  (SCK)  ──┬── TFT SCK
                 ├── SD  SCK
                 ├── CC1101 SCK
                 └── NRF24  SCK
```

---

### 📺 TFT ST7789 172×320

| TFT Pin | ESP32-C5 |
|---------|----------|
| VCC | 3.3V |
| GND | GND |
| CS | GPIO 23 |
| DC | GPIO 24 |
| BL | GPIO 25 |
| RST | Không nối (RST = -1) |
| MOSI | GPIO 7 |
| SCK | GPIO 6 |
| MISO | GPIO 2 |

> 💡 Thêm tụ 100nF gốm giữa VCC và GND, đặt sát module

---

### 💾 SD Card Module

| SD Pin | ESP32-C5 |
|--------|----------|
| VCC | 3.3V |
| GND | GND |
| CS | GPIO 10 |
| MOSI | GPIO 7 |
| MISO | GPIO 2 |
| SCK | GPIO 6 |

> 💡 Thêm tụ 100nF gốm giữa VCC và GND

---

### 📡 CC1101 868MHz

| CC1101 Pin | ESP32-C5 | Ghi chú |
|-----------|----------|---------|
| VCC | SPDT Switch ON-1 | Qua switch |
| GND | GND | |
| MOSI | GPIO 7 | SPI shared |
| SCLK | GPIO 6 | SPI shared |
| MISO | GPIO 2 | SPI shared |
| GDO2 | Không nối | Firmware ghi NC |
| GDO0 | GPIO 8 | Interrupt TX/RX |
| CSN | GPIO 9 | Chip Select |
| ANT | Antenna | Dây 8.2cm hoặc SMA |

> 💡 Thêm tụ 100nF gốm giữa VCC và GND

---

### 📶 NRF24L01+ PA/LNA 2.4GHz

| NRF24 Pin | Số chân | ESP32-C5 | Ghi chú |
|----------|---------|----------|---------|
| GND | 1 | GND | |
| VCC | 2 | SPDT Switch ON-2 | Qua switch |
| CE | 3 | GPIO 8 | Chung GDO0 CC1101 |
| CSN | 4 | GPIO 9 | Chip Select |
| SCK | 5 | GPIO 6 | SPI shared |
| MOSI | 6 | GPIO 7 | SPI shared |
| MISO | 7 | GPIO 2 | SPI shared |
| IRQ | 8 | Không nối | Firmware dùng polling |
| GND | 9 | GND | |
| GND | 10 | GND | |

> ⚠️ **BẮT BUỘC**: Thêm tụ **100µF hóa + 100nF gốm song song** giữa VCC và GND — NRF24 PA/LNA tiêu thụ ~120mA khi TX, sẽ reset ngẫu nhiên nếu thiếu!

---

### 🔌 SPDT Switch — Chọn CC1101 hoặc NRF24

```
3.3V ──── COM (chân giữa)
           ├── ON-1 ──── CC1101 VCC
           └── ON-2 ──── NRF24  VCC
```

| Vị trí Switch | Kết quả |
|--------------|---------|
| Gạt sang ON-1 | CC1101 bật, NRF24 tắt (0mA) |
| Gạt sang ON-2 | NRF24 bật, CC1101 tắt (0mA) |

> ✅ SPI bus (MOSI/MISO/SCK/CS/CE) vẫn nối thẳng cả 2 module — module mất nguồn sẽ tự động high-impedance, không gây conflict

---

### 🔴 IR Transmitter (38KHz)

| IR TX Pin | ESP32-C5 |
|----------|----------|
| VCC | 3.3V |
| GND | GND |
| DAT | GPIO 3 |

---

### 🔵 IR Receiver (38KHz)

| IR RX Pin | ESP32-C5 |
|----------|----------|
| VCC | 3.3V |
| GND | GND |
| OUT | GPIO 26 |

---

### 🎮 Buttons (3 nút)

| Button | GPIO | Nối với |
|--------|------|---------|
| UP | GPIO 0 | GND |
| DOWN | GPIO 1 | GND |
| SELECT | GPIO 28 | GND |

> Pull-up internal được bật trong firmware. Nhấn = LOW.

---

### 🔋 LiPo Battery + Module Sạc Boost 5V

```
USB-C 5V ──── Module IN+
USB-C GND──── Module IN-
Module B+ ──── LiPo (+)
Module B- ──── LiPo (-) = GND
Module OUT+ ── Công tắc nguồn SW ── ESP32-C5 5V/BAT
Module OUT- ── GND
```

> 💡 Thêm tụ 100µF hóa + 100nF gốm tại OUT+ → GND để ổn định nguồn

---

### 📊 Tụ Decoupling — Đặt sát chân VCC/GND mỗi module

| Vị trí | Tụ cần dùng | Ưu tiên |
|--------|------------|---------|
| ESP32-C5 VCC | 100µF hóa + 100nF gốm | ⭐⭐⭐ |
| NRF24 VCC | 100µF hóa + 100nF gốm | ⭐⭐⭐ BẮT BUỘC |
| Module OUT+ | 100µF hóa + 100nF gốm | ⭐⭐ |
| CC1101 VCC | 100nF gốm | ⭐⭐ |
| TFT VCC | 100nF gốm | ⭐ |
| SD Card VCC | 100nF gốm | ⭐ |
| IR TX VCC | 100nF gốm | ⭐ |
| IR RX VCC | 100nF gốm | ⭐ |

> ⚠️ Tụ hóa có phân cực: chân dài (+) vào VCC, chân ngắn (-) vào GND
> ✅ Tụ gốm 104 không phân cực: hàn chiều nào cũng được

---

### 📌 Bảng GPIO Tổng Hợp

| GPIO | Chức năng |
|------|-----------|
| 0 | Button UP → GND |
| 1 | Button DOWN → GND |
| 2 | SPI MISO (shared) |
| 3 | IR TX DAT |
| 6 | SPI SCK (shared) |
| 7 | SPI MOSI (shared) |
| 8 | CC1101 GDO0 / NRF24 CE |
| 9 | CC1101 CS / NRF24 SS |
| 10 | SD Card CS |
| 23 | TFT CS |
| 24 | TFT DC |
| 25 | TFT BL (Backlight) |
| 26 | IR RX OUT |
| 28 | Button SELECT → GND |

---

## 🖼️ Boot Screen

Đặt file ảnh vào thư mục gốc SD card:

```
SD:/
├── boot.jpg   ← JPG (khuyến nghị, nhẹ hơn)
└── boot.png   ← PNG (chất lượng cao hơn)
```

- Kích thước: **320×172 pixels** (landscape)
- Hiển thị trong 7 giây khi khởi động
- Nhấn bất kỳ nút nào để bỏ qua

---

## 🌐 Evil Portal Templates

Đặt file HTML vào SD card để dùng với Evil Portal:

```
SD:/
├── evil_twin_viettel.html    ← Giả mạo ISP Viettel
├── evil_twin_router.html     ← Giả mạo router admin
└── upgrade_portal.html       ← "Nâng cấp mạng miễn phí"
```

---

## 🔨 Build & Flash

```bash
# Build
pio run -e esp32-c5-tft

# Upload
pio run -e esp32-c5-tft -t upload
```

---

## 📦 Flash từ BIN file

### Bước 1 — Tải file BIN

Vào trang **[Releases](https://github.com/nhatowo/bruce-esp32c5-anhnhat07-releases/releases)**, chọn bản mới nhất:

| File | Địa chỉ flash |
|------|--------------|
| `bootloader-vX.X.X.bin` | `0x2000` (ESP32-C5) |
| `partitions-vX.X.X.bin` | `0x8000` |
| `firmware-vX.X.X.bin` | `0x10000` |
| `Bruce-esp32-c5-tft-vX.X.X.bin` | `0x0` (gộp — flash một lần) |

---

### Bước 2 — Cài esptool

```bash
pip install esptool
```

---

### Bước 3 — Flash

> ⚠️ Thay `COM3` bằng cổng COM thực tế của bạn (Windows: Device Manager → Ports)
> ⚠️ Thay `vX.X.X` bằng version đã tải

**Full flash — file gộp (khuyến nghị):**
```bash
esptool.py --chip esp32c5 --port COM3 --baud 921600 write_flash -z 0x0 Bruce-esp32-c5-tft-vX.X.X.bin
```

**Full flash — từng file (lần đầu hoặc sau khi đổi partitions):**
```bash
esptool.py --chip esp32c5 --port COM3 --baud 921600 write_flash -z ^
  0x2000  bootloader-vX.X.X.bin ^
  0x8000  partitions-vX.X.X.bin ^
  0x10000 firmware-vX.X.X.bin
```

**Chỉ update firmware (nhanh hơn):**
```bash
esptool.py --chip esp32c5 --port COM3 --baud 921600 write_flash -z ^
  0x10000 firmware-vX.X.X.bin
```

**Linux/macOS** (thay `^` bằng `\`):
```bash
esptool.py --chip esp32c5 --port /dev/ttyUSB0 --baud 921600 write_flash -z \
  0x2000  bootloader-vX.X.X.bin \
  0x8000  partitions-vX.X.X.bin \
  0x10000 firmware-vX.X.X.bin
```

> 💡 Nếu lỗi kết nối: giữ nút **BOOT** trên board khi cắm USB, thả sau khi esptool bắt đầu upload

---

## 📝 Credits

- Original Bruce: [pr3y/Bruce](https://github.com/pr3y/Bruce)
- Custom mod: **AnhNhat07**
- Hardware: Ebyte E101-C5WN8-XS + ST7789 1.47"

---

## 📧 Liên Hệ

- **GitHub**: [nhatowo/bruce-esp32c5-anhnhat07](https://github.com/nhatowo/bruce-esp32c5-anhnhat07)
- **Facebook**: [fb.com/MYNAMEISNHAT07](https://fb.com/MYNAMEISNHAT07)
- **Telegram**: [@AnhNhat07](https://t.me/AnhNhat07)

---

## ⚠️ Tuyên Bố Miễn Trừ Trách Nhiệm

- Tác giả (**AnhNhat07**) **KHÔNG chịu trách nhiệm** về bất kỳ hành vi sử dụng sai mục đích nào
- Người dùng **TỰ CHỊU TRÁCH NHIỆM HOÀN TOÀN** về mọi hành động của mình
- Firmware được cung cấp **"NGUYÊN TRẠNG"** (AS-IS) không có bảo hành
- Tác giả không khuyến khích, hỗ trợ hoặc tham gia vào bất kỳ hoạt động bất hợp pháp nào

### 🎓 Mục Đích Sử Dụng

Firmware này được phát triển **CHỈ** cho các mục đích sau:
- 🔬 **Nghiên cứu bảo mật**: Tìm hiểu cách thức hoạt động của các giao thức WiFi
- 🛡️ **Kiểm tra bảo mật**: Đánh giá độ an toàn của mạng WiFi **do bạn sở hữu**
- 📚 **Giáo dục**: Học tập về wireless security và penetration testing
- 🏢 **Pentest hợp pháp**: Kiểm tra bảo mật cho khách hàng **có hợp đồng rõ ràng**

### 🚫 Nghiêm Cấm

**KHÔNG ĐƯỢC** sử dụng firmware này để:
- ❌ Tấn công mạng WiFi của người khác mà không có sự cho phép
- ❌ Đánh cắp thông tin cá nhân, mật khẩu, dữ liệu của người khác
- ❌ Gây gián đoạn dịch vụ mạng công cộng hoặc doanh nghiệp
- ❌ Bất kỳ hành vi vi phạm pháp luật nào liên quan đến an ninh mạng
