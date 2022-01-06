# Devices
![Devices](https://user-images.githubusercontent.com/76865553/136703701-40771976-3d25-4fd7-8c63-d51d5b498224.jpeg)

Trong Phần này, bạn có thể:

- Thêm ID bố cục âm thanh
- Khắc phục sự cố USB
- Thêm ID giả để kích hoạt các thiết bị không được hỗ trợ khác
- Thêm hoặc sửa đổi Thiết bị (thông qua `Thiết bị`> 'Thuộc tính`)
- Thêm Framebuffer-Patches (thông qua `Devices`>` Properties`)

## FakeID
Một nhóm các thông số để che các thiết bị của bạn dưới dạng những thông số được macOS hỗ trợ nguyên bản.

![Fake_ID](https://user-images.githubusercontent.com/76865553/136476522-a502bb4d-a183-4ef0-9516-956bb9926f99.png)

**Examples**:

- ** AMD RadeonHD 7850 ** có DeviceID = `0x6819`, không được OSX 10.8 hỗ trợ, nhưng thay vào đó nó hỗ trợ DeviceID =` 0x6818`. Vì vậy, hãy thêm `0x6818` vào trường` ATI` để thay đổi ID của nó. Tiếp theo, cần phải tiêm giả này bằng cách nào đó. Đối với card màn hình, có hai cách: trong "Đồ họa", bật `Inject ATI` hoặc trong" ACPI ", bật` FixDisplay`.
- ** NVidia GTX 660 **: DeviceID = `0x1183`. Thẻ hoạt động nhưng không có `AGPM '(Apple Graphics Power Management) cho nó. Thêm ID `0x0FE0` vào trường` NVidia` và bật bản vá lỗi 'FixDisplay` để đưa nó vào `DSDT`.
- ** WiFi ** - thẻ Dell Wireless 1595, có DeviceID = `0x4315`, nhưng thẻ thật là của Broadcom, hỗ trợ chip 4312, 4331 và một số loại khác. Vì vậy, hãy nhập ID thiết bị cho một thiết bị được hỗ trợ và kích hoạt bản vá lỗi `FixAirport` trong phần` ACPI` để đưa nó vào trong `DSDT`.
- Thẻ mạng ** Marvell 80E8056 ** thông thường (DeviceID = `0x4353`) không hoạt động, nhưng nó hoạt động với trình điều khiển AppleYukon2 nếu bạn thay đổi ID hoán đổi thành` 0x4363`. Kết hợp với Bản vá DSDT `FixLan`.
- ** `IMEI` ** - Thiết bị này hoạt động với ** Intel ** ** HD3000 ** và ** HD4000 **. Tuy nhiên, không chắc rằng chipset của bạn có ID chính xác. Các thay thế như sau (bật Fix `AddIMEI`):

- SandyBridge = `0x1C3A8086 '
- IvyBridge = `0x1E3A8086 '
- Haswell = `0x8C3A8086`

Mặt nạ này hoạt động trong hai trường hợp: khi tiêm hoặc với miếng dán `DSDT`. Tuy nhiên, nếu bạn không muốn tiêm đầy đủ theo cách Clover thực hiện, bạn có thể sử dụng tùy chọn `NoDefaultProperties`.

## USB
![USB](https://user-images.githubusercontent.com/76865553/136476548-3c37da88-f0ad-43e2-a431-1cec1a9ec1af.png)

### Inject
Chèn các thuộc tính USB nếu được bật. Để lại trạng thái vô hiệu hóa nếu bạn muốn tự mình tiêm chúng qua`Properties`.

### Add ClockID

Bật để ngăn bộ điều khiển USB vô tình đánh thức hệ thống. Nếu bạn muốn đánh thức để đánh thức hệ thống qua chuột USB, hãy tắt nó. Nhưng hãy chuẩn bị rằng máy tính của bạn sẽ tự động thức dậy, ví dụ: từ camera tích hợp, v.v.

### FixOwnership
Để bộ điều khiển USB hoạt động trong macOS, trước tiên nó phải được ngắt kết nối khỏi BIOS, trước khi khởi động nhân Darwin. Vì điều này chỉ được yêu cầu cho khởi động kế thừa, tùy chọn này bị tắt theo mặc định - nó không bắt buộc đối với khởi động UEFI.

### HighCurrent
Tăng dòng điện trên bộ điều khiển USB này để sạc Thiết bị - bị tắt theo mặc định.

### NameEH00
Không có thông tin có sẵn trong tài liệu.

## Audio
![audio](https://user-images.githubusercontent.com/76865553/136476581-c7714448-69f9-4c3b-bfa6-13aef9483a79.png)

#### Inject
Chèn Layout-ID. Kết hợp với `AppleALC.kext`. Xem tài liệu cho codec này để chọn đúng. Không cần thiết nếu sử dụng `VoodooHDA.kext`.

Các tùy chọn khác có sẵn từ menu thả xuống:

- `Không`: Không có gì được tiêm. Ví dụ: nếu bạn tự đặt Layout-ID thông qua Device `Properties`
- `Phát hiện`: Tự động phát hiện một card âm thanh đã cài đặt để sử dụng ID của nó làm bố cục. Thực ra vô nghĩa, nhưng rất phổ biến.

#### AFGLowPowerState
Ảnh hưởng đến trình điều khiển `AppleHDA` và dường như giải quyết vấn đề với các cú nhấp chuột và bật lên sau khi thức dậy. Tuy nhiên, có rất ít bằng chứng về hiệu quả của bản vá.

#### ResetHDA
Khởi tạo codec âm thanh, nếu được bật. Hành vi này có thể được quan sát thấy sau khi khởi động lại từ Windows sang Mac. Trong OpenCore, tính năng này có tên là `ResetTrafficClass '(UEFI> Audio)

## Properties
Đây là lúc mà Clover và đặc biệt là Cấu hình Clover trở nên thực sự khó hiểu! Vì thuật ngữ `Thuộc tính` được sử dụng 3 lần trong 3 ngữ cảnh khác nhau.

Nói chung, sử dụng tab `Thuộc tính` (bên cạnh tab 'Tùy ý`) là phương pháp được khuyến nghị để chèn các thuộc tính của thiết bị (dựa trên đường dẫn PCI của thiết bị) như các bản vá bộ đệm Khung và mọi thứ khác mà bạn quen thuộc từ` Phần DeviceProperties trong OpenCore. Nó hoạt động hoàn toàn giống nhau.

### AddProperties
![AddProperties1](https://user-images.githubusercontent.com/76865553/136595982-7a5af1ab-bd37-489c-864b-4a7d9d41be29.png)

Việc thêm các mục vào danh sách này sẽ tạo ra một `<Array>` `AddProperties` và` <Dictionary> `cho thiết bị được liệt kê. Đây là cách cấu trúc thực tế của mảng trông như thế nào khi được xem bằng trình chỉnh sửa plist:
![AddProperties2](https://user-images.githubusercontent.com/76865553/136596168-ef38a5a9-e768-4ccd-805f-c5c4297435fb.png)

Giá trị phải được nhập dưới dạng `<dữ liệu>` hoặc `chuỗi hex`. Vì vậy, thay vì các giá trị alphanunercsl (ABC ...), bạn phải sử dụng hex (0x414243). Chuyển đổi qua PlistEditor hoặc Xcode.
Khóa Thiết bị đầu tiên xác định thuộc tính này sẽ được thêm vào thiết bị nào.

### Properties (Hex)

![Properties_Hex](https://user-images.githubusercontent.com/76865553/136596456-88ad496b-8a38-44e9-b4ed-7f2c50573303.png)

Trường này tạo một chuỗi đơn giản trong cấu hình trong Phần `Thiết bị` nếu giá trị hex được nhập:

![PropertiesHex2](https://user-images.githubusercontent.com/76865553/136596474-e3ce6d35-3f93-4194-b9e0-02a0231d470b.png)

Nhưng ngay sau khi bạn thêm Thiết bị vào trong Tab `Thuộc tính` thực tế (bên cạnh` Tùy ý`), khóa này sẽ bị xóa.

## Arbitrary
![Arbitrary](https://user-images.githubusercontent.com/76865553/136480147-879718e6-81eb-474d-a443-a13e0b56988a.png)

Phần `Arbitrary` là một loạt các từ điển, mỗi từ điển tương ứng với một thiết bị có địa chỉ PCI nhất định. Để mô tả từng thiết bị, một mảng `CustomProperties` bao gồm các cặp` Key` / `Value` được sử dụng. Có thể tắt các Thuộc tính này bằng cách đánh dấu vào hộp kiểm `Đã tắt`. Bạn có thể bật hoặc tắt động một thuộc tính trong menu Cỏ ba lá.

- `Key` **must** be a `<string>`. 
- `Value` **can** be a `<string>`, `<integer>` or `<data>`.

### Inject
Nếu được bật, tất cả phần chèn nội bộ sẽ được thay thế bằng cách nhập Thuộc tính chuỗi đơn, tương ứng với phần chèn APPLE_GETVAR_PROTOCOL của Apple với GUID = {0x91BD12FE, 0xF6C3, 0x44FB, {0xA5, 0xB7, 0x51, 0x22, 0xAB, 0x30, 0x3A, 0xE0} ; được sử dụng trên máy Mac thực. Các tin tặc cũ gọi nó là `EFIstrings`.

### NoDefaultProperties
Trong trường hợp này, dòng cho tiêm được tạo, nhưng chưa chứa bất kỳ thuộc tính nào. Ví dụ: thuộc tính này có thể là một `FakeID`. Cách chèn `FakeID` này đã lỗi thời, vì vậy hãy sử dụng tab` Properties` để thay thế.

### UseIntelHDMI
Thông số này ảnh hưởng đến đặc tính tiêm của âm thanh truyền qua HDMI, cũng như bản vá `DSDT`. Tuy nhiên, cả trình điều khiển âm thanh VoodooHDA và AppleHDA, đều không hoạt động hoàn toàn với Đầu ra HDMI. Theo thông tin mới, VoodooHDA chỉ hoạt động với đầu ra HDMI của NVIDIA và đối với AMD, Apple đã tạo trình điều khiển `AppleGFXHDA.kext` mới trong hệ thống 10.13+.

### HDMIInjection
Tắt hoàn toàn việc đưa các thuộc tính của thiết bị HDMI. Bắt đầu với r3262, có một cách mới để đưa các thuộc tính của thiết bị vào không phải theo tên mà theo vị trí của chúng trên Bus PCI.

### ForceHPET
Force bật Bộ hẹn giờ sự kiện có độ chính xác cao trên các hệ thống không có tùy chọn trong BIOS để kích hoạt nó.

### SetIntelBacklight
Khóa được giới thiệu trong r3298. Trong các hệ thống trước đây, độ sáng màn hình được điều khiển bởi `IntelBacklight.kext` hoặc` ACPIBacklight.kext`, nhưng chúng không hoạt động trong El Capitan. Nhưng hóa ra việc này rất dễ thực hiện trong Clover ở giai đoạn khởi động hệ thống, vì vậy không cần thêm bánh nào. </br>
** LƯU Ý **: Điều này không hoạt động với các phiên bản macOS hiện tại. Sử dụng ổ đĩa `SSDT-PNLF.aml`. Bạn có thể tìm thấy một trong Thư mục Mẫu của Gói OpenCore.

### LANInjection
Theo mặc định, thuộc tính tích hợp được đưa vào cho NIC. Tham số này có thể được sử dụng để vô hiệu hóa tiêm.