# HƯỚNG DẪN DỰNG FILE CISCO PACKET TRACER (.PKT) TRONG 10 PHÚT

**Đề tài 1: Thiết kế và triển khai mạng LAN cho phòng thực hành 30 máy**

- **Dải mạng sử dụng:** `192.168.10.0/26` (`Subnet Mask: 255.255.255.192`)
- **Tên file lưu nộp:** `FIT4006_Lop_Nhom_DeTai1.pkt`

---

## BƯỚC 1: KÉO THẢ THIẾT BỊ LÊN WORKSPACE

Mở phần mềm **Cisco Packet Tracer**, kéo các thiết bị sau ra màn hình làm việc và đổi tên hiển thị (**Display Name**) đúng chuẩn báo cáo:

1. **Router (1 thiết bị):**
   - Vào `Network Devices` $\rightarrow$ `Routers` $\rightarrow$ Chọn **Cisco 2911** (hoặc **1941**).
   - Đặt tên hiển thị: `R-LAB-GW`.
2. **Switch (2 thiết bị):**
   - Vào `Network Devices` $\rightarrow$ `Switches` $\rightarrow$ Chọn **2960-24TT** (lấy 2 chiếc).
   - Đặt tên hiển thị chiếc bên trái là `SW-LAB-01`, chiếc bên phải là `SW-LAB-02`.
3. **Máy chủ và Máy in dùng chung (2 thiết bị):**
   - Vào `End Devices` $\rightarrow$ `End Devices`:
     - Kéo **1 Server** đặt cạnh `SW-LAB-01`, đổi tên thành `SRV-LAB-01`.
     - Kéo **1 Printer** đặt cạnh `SW-LAB-01`, đổi tên thành `PRN-LAB-01`.
4. **30 Máy tính thực hành (30 PC):**
   - Mẹo tạo nhanh: Giữ phím `Ctrl` rồi click vào biểu tượng **PC** trong `End Devices`, sau đó click 30 lần trên màn hình (15 máy bên trái dưới `SW-LAB-01`, 15 máy bên phải dưới `SW-LAB-02`).
   - Đặt tên từ `PC01` đến `PC15` ở Khu vực A (bên trái) và từ `PC16` đến `PC30` ở Khu vực B (bên phải).

---

## BƯỚC 2: ĐẤU NỐI CÁP MẠNG CHÍNH XÁC THEO BẢNG PORT MAPPING

Vào mục **Connections** (biểu tượng tia sét):

| STT | Thiết bị nguồn                 |      Cổng nguồn      |              Loại cáp              | Thiết bị đích |             Cổng đích             | Ghi chú                               |
| :-: | :-------------------------------- | :--------------------: | :-----------------------------------: | :---------------- | :----------------------------------: | :------------------------------------- |
|  1  | `R-LAB-GW`                      | `GigabitEthernet0/0` | Copper Straight-Through (Cáp thẳng) | `SW-LAB-01`     |        `GigabitEthernet0/1`        | Đường Uplink lên Gateway           |
|  2  | `SW-LAB-01`                     | `GigabitEthernet0/2` |    Copper Cross-Over (Cáp chéo)    | `SW-LAB-02`     |        `GigabitEthernet0/1`        | Liên kết Trunk 802.1Q giữa 2 Switch |
|  3  | `SRV-LAB-01`                    |   `FastEthernet0`   |        Copper Straight-Through        | `SW-LAB-01`     |         `FastEthernet0/1`         | Kết nối Máy chủ học liệu         |
|  4  | `PRN-LAB-01`                    |   `FastEthernet0`   |        Copper Straight-Through        | `SW-LAB-01`     |         `FastEthernet0/2`         | Kết nối Máy in mạng                |
|  5  | `PC01` $\rightarrow$ `PC15` |   `FastEthernet0`   |        Copper Straight-Through        | `SW-LAB-01`     | `Fa0/3` $\rightarrow$ `Fa0/17` | 15 PC thực hành Khu A                |
|  6  | `PC16` $\rightarrow$ `PC30` |   `FastEthernet0`   |        Copper Straight-Through        | `SW-LAB-02`     | `Fa0/1` $\rightarrow$ `Fa0/15` | 15 PC thực hành Khu B                |

---

## BƯỚC 3: COPY - PASTE SCRIPT CẤU HÌNH CHO ROUTER VÀ 2 SWITCH

Mở thư mục `Scripts_Cau_Hinh_CLI` đã tạo sẵn trong `E:\Đề tài 1\Scripts_Cau_Hinh_CLI\`:

1. **Cấu hình `R-LAB-GW`:**
   - Click vào `R-LAB-GW` $\rightarrow$ Chọn tab **CLI**.
   - Nếu xuất hiện câu hỏi `Would you like to enter the initial configuration dialog? [yes/no]:`, gõ `no` rồi nhấn **Enter**.
   - Mở file `01_Cau_hinh_Router_R-LAB-GW.txt`, chọn toàn bộ (`Ctrl + A`), Copy (`Ctrl + C`) và bấm nút **Paste** trong tab CLI của Packet Tracer.
2. **Cấu hình `SW-LAB-01`:**
   - Click vào `SW-LAB-01` $\rightarrow$ Chọn tab **CLI** $\rightarrow$ Nhấn **Enter**.
   - Mở file `02_Cau_hinh_Switch_SW-LAB-01.txt`, Copy toàn bộ và **Paste** vào cửa sổ CLI của `SW-LAB-01`.
3. **Cấu hình `SW-LAB-02`:**
   - Click vào `SW-LAB-02` $\rightarrow$ Chọn tab **CLI** $\rightarrow$ Nhấn **Enter**.
   - Mở file `03_Cau_hinh_Switch_SW-LAB-02.txt`, Copy toàn bộ và **Paste** vào cửa sổ CLI của `SW-LAB-02`.

---

## BƯỚC 4: CẤU HÌNH ĐỊA CHỈ IP CHO SERVER, PRINTER VÀ 30 PC

### 4.1. Cấu hình Máy chủ `SRV-LAB-01` (IP Tĩnh + Dịch vụ DNS/Web)

1. Click `SRV-LAB-01` $\rightarrow$ Tab **Desktop** $\rightarrow$ **IP Configuration** $\rightarrow$ Chọn **Static**:
   - **IPv4 Address:** `192.168.10.5`
   - **Subnet Mask:** `255.255.255.192`
   - **Default Gateway:** `192.168.10.1`
   - **DNS Server:** `192.168.10.5`
2. Chuyển sang tab **Services** của `SRV-LAB-01`:
   - Chọn **HTTP**: Đảm bảo cả **HTTP** và **HTTPS** đều ở trạng thái **On** (có thể sửa dòng chữ trong file `index.html` thành `"CỔNG HỌC LIỆU PHÒNG THỰC HÀNH CNTT - ĐẠI HỌC ĐẠI NAM"`).
   - Chọn **DNS**: Bật **DNS Service** sang **On**, sau đó thêm 2 bản ghi (Type `A Record`):
     - `Name`: `lab.dainam.edu.vn` | `Address`: `192.168.10.5` $\rightarrow$ Nhấn **Add**.
     - `Name`: `printer.dainam.edu.vn` | `Address`: `192.168.10.6` $\rightarrow$ Nhấn **Add**.

### 4.2. Cấu hình Máy in dùng chung `PRN-LAB-01` (IP Tĩnh)

1. Click `PRN-LAB-01` $\rightarrow$ Tab **Config**:
   - Trong mục **GLOBAL** $\rightarrow$ **Settings**: Điền **Gateway** là `192.168.10.1`, **DNS Server** là `192.168.10.5`.
   - Trong mục **INTERFACE** $\rightarrow$ **FastEthernet0**: Chọn **Static**, nhập:
     - **IPv4 Address:** `192.168.10.6`
     - **Subnet Mask:** `255.255.255.192`
     - Tích chọn **Port Status: On**.

### 4.3. Cấu hình 30 PC (`PC01` $\rightarrow$ `PC30`)

- **Cách 1 (Khuyên dùng - Cực nhanh với DHCP):**
  - Mở lần lượt từ `PC01` đến `PC30` $\rightarrow$ Tab **Desktop** $\rightarrow$ **IP Configuration** $\rightarrow$ Tích chọn **DHCP**.
  - Ngay lập tức Router `R-LAB-GW` sẽ cấp tự động địa chỉ từ `192.168.10.11` đến `192.168.10.40` với Subnet Mask `255.255.255.192`, Default Gateway `192.168.10.1`, DNS Server `192.168.10.5`.
- **Cách 2 (Đặt tĩnh nếu muốn cố định từng máy từ `.11` đến `.40`):**
  - Nhập Static IP theo đúng bảng quy hoạch: `PC01` = `192.168.10.11`, `PC02` = `192.168.10.12`, ..., `PC30` = `192.168.10.40`, Subnet Mask `255.255.255.192`, Default Gateway `192.168.10.1`, DNS Server `192.168.10.5`.

---

## BƯỚC 5: LƯU CẤU HÌNH VÀ KIỂM THỬ

1. Bấm nút **Fast Forward Time** (`Alt + D`) ở góc dưới bên trái Packet Tracer 2 lần để giao thức Spanning-Tree chuyển toàn bộ các cổng sang màu xanh lá.
2. Mở file `04_Lenh_Kiem_Thu_Show_Ping_Telnet.txt` để chạy các lệnh kiểm thử và đối chiếu với Chương 3 trong Báo cáo Word.
3. Nhấn `Ctrl + S` và lưu file với tên `FIT4006_Lop_Nhom_DeTai1.pkt`.
