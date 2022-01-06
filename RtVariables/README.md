# RtVariables
![RTvarsnu](https://user-images.githubusercontent.com/76865553/140332564-944c61eb-6168-4a12-b580-0f0744fd4fdf.png)

Hai thông số sau nhằm cho phép đăng ký dịch vụ iMessage.
Bắt đầu từ Clover r1129, các tham số được lấy từ SMBiOS và không cần nhập vào đây.

## ROM
Các chữ số cuối của `SmUUID` hoặc` Địa chỉ MAC`. Clover có thể phát hiện Địa chỉ MAC của Bộ điều hợp Ethernet của bạn nếu chọn `UseMacAddr0`. Điều này không hiệu quả với tất cả mọi người, vì vậy hãy thử nghiệm nó.

## MLB
** MLB ** = BoardSerialNumber

## BooterConfig
Đây là các mặt nạ bit để đặt các cờ khởi động khác nhau. Không có thêm thông tin nào trong sách hướng dẫn về chúng. Nhưng nếu bạn muốn biết giá trị `0x28` cho` BooterConfig` đến từ đâu, hãy kiểm tra bảng tiếp theo.

### Bitfields cho cờ boot-arg
|Bit| Flag Name | HEX Value  | Default in r5142
|:---:|-----------|-----------:|:---------------:|
|0|Reboot On Panic    | 0x1|
|1|Hi DPI             | 0x2|
|2|Black Screen       | 0x4|
|3|CSR Active Config  | 0x8|  x|
|4|CSR Pending Config | 0x10|
|5|CSR Boot           | 0x20| x|
|6|Black Background   | 0x40|

** LƯU Ý **: Trong hầu hết các trường hợp, bạn không phải thay đổi bất kỳ điều gì ở đây. Nhưng nếu bạn làm vậy, bạn nên biết chính xác những gì bạn đang làm và tại sao! Bạn cũng có thể thay đổi các cờ này từ menu Tùy chọn trong GUI của Bootloader (Tùy chọn> Tham số hệ thống> bootargs> Cờ). Nhưng trong trường hợp này, các cài đặt được áp dụng chỉ được áp dụng tạm thời cho lần khởi động tiếp theo.

## CsrActiveConfig
Với việc phát hành macOS ElCapitan vào năm 2015, một tính năng bảo mật mới đã được giới thiệu: Bảo vệ toàn vẹn hệ thống (SIP). Theo mặc định, SIP được bật (`` 0x000`) và không cho phép bạn tải kexts hoặc cài đặt các tiện ích hệ thống của bạn. Để vô hiệu hóa nó, Clover cung cấp cho bạn tùy chọn thiết lập mới trong NVRAM.

### Cờ cho Cài đặt Bảo mật
Giá trị mặc định cho `CsrActiveConfig` trong Clover r5142 hiện là` 0xA87`, bao gồm các cờ được bật sau:

| Bit | Cờ Tên | Giá trị HEX | Mặc định trong r5142
|: -: | ----------- | ----------: |: ---------------: |
| 0 | CSR_ALLOW_UNTRUSTED_KEXTS | 0x1 | x
| 1 | CSR_ALLOW_UNRESTRICTED_FS | 0x2 | x
| 2 | CSR_ALLOW_TASK_FOR_PID | 0x4 | x
| 3 | CSR_ALLOW_KERNEL_DEBUGGER | 0x8 |
| 4 | CSR_ALLOW_APPLE_INTERNAL | 0x10 |
| 5 | CSR_ALLOW_UNRESTRICTED_DTRACE | 0x20 |
| 6 | CSR_ALLOW_UNRESTRICTED_NVRAM | 0x40 |
| 7 | CSR_ALLOW_DEVICE_CONFIGURATION | 0x80 | x
| 8 | CSR_ALLOW_ANY_RECOVERY_OS | 0x100 |
| 9 | CSR_ALLOW_UNAPPROVED_KEXTS | 0x200 | x
| 10 | CSR_ALLOW_EXECUTABLE_POLICY_OVERRIDE | 0x400 |
| 11 | CSR_ALLOW_UNAUTHENTICATED_ROOT | 0x800 | x

** LƯU Ý **: `0xA87` là một mặt nạ bit 12 bit và như vậy, chỉ hợp lệ cho macOS 11 và 12. Vì vậy, nếu bạn đang sử dụng Phiên bản macOS cũ hơn, hãy sử dụng ** CloverCalcs ** Spreadsheed có thể được tìm thấy bên trong[**Xtras Section**](https://github.com/5T33Z0/Clover-Crate/tree/main/Xtras) để tính toán của riêng bạn.

Bạn cũng có thể thay đổi các cờ này từ menu Tùy chọn trong GUI của Bộ nạp khởi động (Tùy chọn> Tham số hệ thống> Bảo vệ toàn vẹn hệ thống). Nhưng trong trường hợp này các cài đặt chỉ được áp dụng tạm thời trong lần khởi động tiếp theo.

### Các giá trị được đề xuất để tắt Bảo vệ tính toàn vẹn của hệ thống
: cảnh báo: Không khuyến khích sử dụng SIP!

| macOS Version     | Bitmask Size  | CsrActiveConfig |
|------------------:|:-------------:|:---------------:|
| macOS 11/12       | 12 bits       |`0xFEF`
| macOS 10.14/10.15 | 11 bits       |`0x7FF`
| macOS 10.13       | 10 bits       |`0x3FF`
| macOS 10.12       | 9 bits        |`0x1FF`
| macOS 10.11       | 8 bits        |`0x0FF`

** LƯU Ý **:
Mặc dù không được khuyến khích nhưng vẫn có những trường hợp đặc biệt, bạn cần phải tắt hoàn toàn SIP. Ví dụ: nếu phải vá các trình điều khiển đã gỡ bỏ trong quá trình cài đặt (như đồ họa NVIDIA Kepler hoặc Intel HD4000 trong macOS 12), thì hoàn toàn * bắt buộc * phải tắt SIP thành 1) có thể cài đặt trình điều khiển và 2) để khởi động hệ thống sau đó!

Check the [**Xtras Section**](https://github.com/5T33Z0/Clover-Crate/tree/main/Xtras) to find out thêm về Booter- và CsrActiveConfig và cách tính toán của riêng bạn.

## HWTarget
`HWTarget` là tính năng mới nhất được thêm vào Clover r5140. Nó cho phép Cập nhật hệ thống cho macOS Monterey sử dụng SMBIOS của máy Mac với Chip bảo mật T2. Nó ghi biến `BridgeOSHardwareModel` vào NVRAM, được yêu cầu bởi macOS Monterey.

### Giá trị hợp lệ cho HWTarget
Nếu bạn sử dụng SMBIOS của một trong các kiểu máy Mac được liệt kê bên dưới, hãy sao chép giá trị tương ứng và dán nó vào trường `HWTarget` của` config.plist`.

- MacBookPro15,1 (`J680AP`), 15,2 (` J132AP`), 15,3 (`J780AP`), & 15,4 (` J213AP`)
- MacBookPro16,1 (`J152FAP`), 16,3 (` J223AP`), & 16,4 (`J215AP`)
- MacBookPro16,2 (`J214KAP`)
- MacBookAir8,1 (`J140KAP`) & 8,2 (` J140AAP`)
- MacBookAir9,1 (`J230KAP`)
- Macmini8,1 (`J174AP`)
- iMac20,1 (`J185AP`) & 20,2 (` J185FAP`)
- iMacPro1,1 (`J137AP`)
- MacPro7,1 (`J160AP`)

Để xác nhận rằng tham số đã được đặt, hãy khởi động lại và nhập vào Terminal: `sysctl hw.target`.
Nó sẽ trả về giá trị cho `HWTarget` mà bạn đã nhập trong cấu hình.

**Source**: [**Insanelymac**](https://www.insanelymac.com/forum/topic/284656-clover-general-discussion/?do=findComment&comment=2771041)

**NOTES**: 

- `HWTarget` chỉ được yêu cầu khi sử dụng SMBIOS của máy Mac có chip T2.
- `HWTarget` chỉ cần thiết khi nâng cấp từ Big Sur lên Monterey. Khi macOS Monterey đang chạy, nó có vẻ như không còn được yêu cầu (được cho là).