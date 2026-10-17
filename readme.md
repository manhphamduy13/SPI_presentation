# PHÂN CÔNG THUYẾT TRÌNH: SERIAL PERIPHERAL INTERFACE (SPI)

**Môn học:** Mạch điện điện tử  
**Chủ đề:** SPI – Serial Peripheral Interface  
**Nhóm:** Mạnh – Đạt – Phúc  
**Quy mô đề xuất:** 33 slide, khoảng 30–36 phút (10–12 phút/người), chưa tính hỏi đáp.

## 1. Tổng quan phân công

| Thành viên | Slide | Số lượng | Nội dung phụ trách | Thời lượng |
|---|---|---:|---|---|
| **Mạnh** | 1–11 | 11 | SPI Fundamentals & Working Principles | 10–12 phút |
| **Đạt** | 12–22 | 11 | Timing, Hardware & Software Implementation | 10–12 phút |
| **Phúc** | 23–33 | 11 | Applications, Debugging & Case Studies | 10–12 phút |

**Mục tiêu chung:** Người nghe hiểu SPI từ nguyên lý mạch số và giao thức truyền thông đến triển khai trên MCU, ứng dụng thực tế và phương pháp xử lý lỗi.

---

## 2. Mạnh — SPI Fundamentals & Working Principles (Slide 1–11)

**Mục tiêu:** Giải thích SPI là gì, các tín hiệu và kiến trúc bus, dữ liệu di chuyển qua phần cứng như thế nào, cách một transaction diễn ra.

| Slide | Tiêu đề | Nội dung chính |
|---:|---|---|
| 1 | Introduction to SPI | Tên đề tài, thành viên, mục tiêu và bố cục bài thuyết trình. |
| 2 | Overview of Serial Communication | Parallel vs Serial; Synchronous vs Asynchronous; vị trí của SPI trong truyền thông số. |
| 3 | What is SPI? | Định nghĩa, nguồn gốc, đặc tính serial, synchronous, full-duplex; controller/peripheral. |
| 4 | SPI Architecture | Sơ đồ tổng thể controller–peripheral; bus SPI bốn dây. |
| 5 | SPI Signal Lines | Chức năng và hướng dữ liệu của SCLK, MOSI, MISO, CS/SS/NSS. |
| 6 | Single and Multiple Peripheral Connections | Kết nối 1 hoặc nhiều thiết bị, CS riêng, chia sẻ bus; giới thiệu daisy-chain và lưu ý MISO tri-state. |
| 7 | Internal SPI Hardware Architecture | Shift registers, TX/RX buffers hoặc FIFO, clock generator, vai trò của SPI controller. |
| 8 | How SPI Transfers Data | Dịch bit theo từng clock; MSB-first/LSB-first; full-duplex; ví dụ truyền byte `0xA5`. |
| 9 | SPI Transaction Structure | Phân biệt frame và transaction; CS active-low; command, address, data. |
| 10 | SPI Read and Write Operations | Ghi/đọc thanh ghi; dummy byte/dummy clocks; burst transfer; ví dụ command rồi nhận dữ liệu. |
| 11 | SPI vs I²C, UART and CAN | So sánh clock, dây tín hiệu, tốc độ, cách kết nối và ứng dụng phù hợp. |

**Hình minh họa ưu tiên:**
- Sơ đồ bus SPI và hướng tín hiệu cho slide 4–6.
- Animation/sơ đồ dịch bit giữa hai shift registers cho slide 7–8.
- Timing khái niệm command → dummy → data cho slide 10.

**Trách nhiệm bổ sung:** Thống nhất thuật ngữ toàn nhóm (controller/peripheral, CS/SCLK, frame/transaction) và template slide mở đầu.

**Nguồn tham khảo:** [Analog Devices – Introduction to SPI Interface](https://www.analog.com/en/resources/analog-dialogue/articles/2018/08/13/15/31/introduction-to-spi-interface.html).

---

## 3. Đạt — Timing, Hardware & Software Implementation (Slide 12–22)

**Mục tiêu:** Làm rõ SPI truyền đúng dữ liệu nhờ clock/timing như thế nào, giới hạn điện của đường truyền và cách lập trình, tối ưu driver SPI.

| Slide | Tiêu đề | Nội dung chính |
|---:|---|---|
| 12 | SPI Clock Configuration | CPOL, CPHA; clock idle; leading/trailing edge. |
| 13 | The Four SPI Modes | Bốn mode 0–3; bảng CPOL/CPHA; sampling edge và shifting edge. |
| 14 | SPI Timing Diagrams | Waveform CS, SCLK, MOSI, MISO; minh họa Mode 0 và Mode 3. |
| 15 | Timing Constraints | Setup time, hold time, clock-to-output delay, CS setup/hold; lý do sai một bit. |
| 16 | Clock Frequency & Data Throughput | Công thức bit rate/byte rate, overhead command/address/dummy/CS; ví dụ tính throughput. |
| 17 | SPI Electrical Characteristics | Mức điện áp, VIH/VIL, level shifting, GND chung, high-impedance của MISO. |
| 18 | Signal Integrity & PCB Design | Propagation delay, reflection/ringing, crosstalk, ground return, series termination. |
| 19 | SPI Peripheral Configuration | Clock prescaler, mode, bit order, frame size, CS control, TX/RX FIFO. |
| 20 | SPI Programming Methods | Polling, interrupt, DMA; sơ đồ luồng dữ liệu và ưu/nhược điểm. |
| 21 | Practical SPI Programming | Ví dụ pseudocode hoặc STM32/ESP32: khởi tạo SPI, CS, đọc một thanh ghi. |
| 22 | Performance Optimization | DMA vs polling, gộp transaction, buffer size, CPU overhead, giới hạn throughput. |

**Hình minh họa ưu tiên:**
- Bảng và waveform 4 SPI modes cho slide 13–14.
- Setup/hold time và bài toán timing margin cho slide 15.
- Dạng sóng ringing đo bằng oscilloscope cho slide 18.
- Sơ đồ polling/interrupt/DMA cho slide 20.

**Trách nhiệm bổ sung:** Kiểm tra công thức, ví dụ tính toán, timing diagram và thông số kỹ thuật được trích từ datasheet (phân biệt số giả định với số thực tế).

**Nguồn tham khảo:**
- [Analog Devices – Introduction to SPI Interface](https://www.analog.com/en/resources/analog-dialogue/articles/2018/08/13/15/31/introduction-to-spi-interface.html)
- [ST – AN5543 Guidelines for Enhanced SPI Communication](https://www.st.com/resource/en/application_note/an5543-guidelines-for-enhanced-spi-communication-on-stm32-mcus-and-mpus-stmicroelectronics.pdf)
- [Espressif – ESP-IDF SPI Master Driver](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/peripherals/spi_master.html)

---

## 4. Phúc — Applications, Debugging & Case Studies (Slide 23–33)

**Mục tiêu:** Chỉ ra SPI dùng ở đâu trong sản phẩm thật, giới hạn và lỗi khi triển khai; chứng minh hiểu biết qua một ví dụ/demo thực tế.

| Slide | Tiêu đề | Nội dung chính |
|---:|---|---|
| 23 | SPI Applications Overview | Ứng dụng trong embedded, IoT, robotics, industrial automation, lưu trữ và hiển thị. |
| 24 | Application 1: SPI Flash Memory | SPI NOR Flash, lưu firmware/dữ liệu; ví dụ Winbond W25Q128JV, lệnh đọc/chip ID. |
| 25 | Application 2: Sensors & IMU | Cảm biến gia tốc/gyro; ví dụ Bosch BMI270; đọc thanh ghi và dummy byte. |
| 26 | Application 3: ADC & DAC | SPI kết nối ADC/DAC trong đo lường; ví dụ TI ADS124S08 (ADC); đối chiếu mode/timing trong datasheet. |
| 27 | Application 4: Displays & Storage | TFT LCD/OLED, SD card SPI mode; ảnh hưởng của băng thông lên frame rate và tốc độ ghi/đọc. |
| 28 | Advanced SPI: Dual, Quad & Octal SPI | Multi-I/O, raw throughput, QSPI/OSPI Flash, XIP và giới hạn hiệu năng thực. |
| 29 | Common SPI Communication Errors | Sai mode, CS, bit order; MISO floating, bus contention, overrun/underrun. |
| 30 | Debugging SPI with Instruments | Logic analyzer vs oscilloscope; checklist debug từ nguồn/chân đến timing và waveform. |
| 31 | Real-World SPI Case Studies | DMA complete nhưng bus chưa kết thúc; nhiều task chia sẻ SPI bus và đồng bộ truy cập. |
| 32 | SPI Demonstration / Practical Example | Demo đọc JEDEC ID (`0x9F`) từ SPI NOR Flash hoặc mô phỏng minh bạch; sơ đồ, code, waveform, kết quả. |
| 33 | Conclusion & References | Tóm tắt, ứng dụng nổi bật, những lưu ý kỹ thuật và danh mục tài liệu; chuyển sang Q&A. |

**Ví dụ ứng dụng phải có:**
1. **SPI NOR Flash — Winbond W25Q128JV:** lưu firmware/dữ liệu, Standard/Dual/Quad SPI, ví dụ `0x9F` đọc JEDEC ID.
2. **IMU — Bosch BMI270:** truyền dữ liệu gia tốc/gyro, phân tích cách đọc thanh ghi và dummy byte.
3. **ADC — Texas Instruments ADS124S08:** truyền dữ liệu đo chính xác, cấu hình mode/timing theo datasheet.
4. **Màn hình TFT:** ví dụ `320 × 240 × 2 = 153.600 byte/frame`; ở SPI 40 MHz, thời gian truyền lý tưởng `30,72 ms/frame` (chưa tính overhead).

**Hình minh họa ưu tiên:**
- Ảnh IC và sơ đồ kết nối từng ứng dụng cho slide 24–27.
- So sánh single/quad/octal SPI cho slide 28.
- Capture logic analyzer hoặc oscilloscope cho slide 30–32.

**Trách nhiệm bổ sung:** Tổng hợp references cuối bài, chuẩn bị demo hoặc trace mô phỏng (phải chú thích rõ nếu không phải phép đo thật), kiểm tra tên IC và nội dung datasheet.

**Nguồn tham khảo:**
- [Winbond W25Q128JV Datasheet](https://www.winbond-electronics.com/pdf-7d/w25q128jvsjq.pdf)
- [Bosch BMI270 Datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bmi270-ds000.pdf)
- [TI ADS124S08](https://www.ti.com/product/ADS124S08)
- [Microchip TB3331 – Debugging Serial Interfaces](https://www.microchip.com/en-us/application-notes/tb3331)
- [Linux Kernel SPI Documentation](https://docs.kernel.org/driver-api/spi.html)

---

## 5. Quy chuẩn trình bày chung

- **Ngôn ngữ:** Tiêu đề slide tiếng Anh, thuyết trình và giải thích bằng tiếng Việt; thuật ngữ kỹ thuật thống nhất.
- **Mỗi slide:** Một ý chính; ưu tiên sơ đồ/waveform/ảnh phần cứng thay vì đoạn văn dài.
- **Sơ đồ:** Ghi chú hướng tín hiệu, thiết bị phát clock, vai trò CS; không vẽ timing trái với SPI mode.
- **Ví dụ số:** Ghi rõ giá trị nào là giả định để minh họa và giá trị nào lấy từ datasheet.
- **Nguồn:** Để footnote trên slide có thông số hoặc hình tham khảo; có bảng tài liệu ở slide 33.
- **Chuyển giao:** Mạnh kết thúc bằng lựa chọn SPI và câu hỏi về đồng bộ clock; Đạt kết thúc bằng tối ưu hiệu năng; Phúc liên hệ các nguyên lý đó với ứng dụng và debug thực tế.
- **Q&A:** Cả ba cùng chuẩn bị trả lời cho phần mình và một số câu hỏi chéo.

## 6. Checklist trước khi nộp

- [ ] 33 slide, đúng phân công Mạnh 1–11; Đạt 12–22; Phúc 23–33.
- [ ] Có đầy đủ 4 chân SPI, shift register, full-duplex, CS, read/write/dummy clocks.
- [ ] Có bảng và waveform bốn SPI mode; setup/hold và CS timing.
- [ ] Có ví dụ tính clock, tốc độ và overhead; electrical signal integrity.
- [ ] Có lập trình SPI bằng polling/interrupt/DMA.
- [ ] Có ít nhất 4 nhóm ứng dụng với IC hoặc sản phẩm cụ thể.
- [ ] Có Dual/Quad/Octal SPI, debug và case study thực tế.
- [ ] Có demo hoặc ví dụ transaction có giải thích source/datasheet.
- [ ] Tất cả hình, thông số và tài liệu có ghi nguồn; kiểm tra lại đường dẫn trước khi nộp.
