# I. ACPI

Phần ACPI cung cấp nhiều tùy chọn để ảnh hưởng đến Bảng ACPI của hệ thống nhằm hỗ trợ người dùng làm cho nó tương thích với macOS: từ việc thay thế các ký tự trong `DSDT`, thay đổi tên thiết bị, cho phép các tính năng áp dụng các bản vá. Vì phần này là trung tâm của `config.plist` của bạn, mọi tùy chọn có sẵn đều được giải thích ở đây. Đây là cách nó trông giống như trong Trình cấu hình Clover:

![ACPI_FULL](https://user-images.githubusercontent.com/76865553/135732525-5ad50aa8-6bfe-47b4-82b7-3bc3df8c6f62.png)

Trong suốt chương này, phần này được chia thành các phần nhỏ hơn để dễ tiêu hóa hơn.

## AutoMerge

![Bildschirmfoto](https://user-images.githubusercontent.com/76865553/135732535-ca43869b-4e97-47b2-a490-9c8aa5488fa8.png)

Hợp nhất mọi thay đổi `DSDT` và` SSDT` từ `/ACPI/patched` với các bảng ACPI hiện có.

Nếu được đặt thành `true`, nó sẽ thay đổi cách áp dụng các tệp .aml trong` ACPI / patched`. Thay vì nối các bảng này vào cuối `DSDT`, chúng sẽ thay thế các bảng hiện có, nếu chữ ký, chỉ mục và` OemTableIds` của chúng khớp với các bảng OEM hiện có. Nói cách khác, chúng sẽ được hợp nhất với các bảng ACPI hiện có.

Với chức năng này - cũng như với `DSDT` - bạn có thể sửa các SSDT riêng lẻ (hoặc các bảng khác) chỉ bằng cách thêm (các) tệp đã sửa vào` ACPI/patched`. Không cần phải thao tác với `DropOem` hoặc` DropTables`. Thứ tự ban đầu được giữ nguyên. Ánh xạ cho `SSDT` dựa trên việc đặt tên, trong đó quy ước đặt tên được sử dụng bởi trình trích xuất F4 trong menu bộ tải được sử dụng để xác định vị trí` SSDT` trong `DSDT`.

Ví dụ: nếu thư mục `ACPI/origin` của bạn chứa` SSDT-6-SaSsdt.aml`, bạn chỉ cần sửa nó và đưa nó trở lại trong `ACPI/patched`, thay thế bảng gốc. Điều này cũng hoạt động nếu bạn đặt nó trong `ACPI /vá lỗi` là` SSDT-6.aml`. Vì một số bộ ACPI OEM không sử dụng văn bản duy nhất trong trường id bảng OEM, nên Clover sử dụng cả id bảng OEM và số là một phần của tên tệp để định vị bản gốc trong `XDST`. Miễn là bạn tuân theo các tên được cung cấp trong `ACPI/origin`, bạn sẽ ổn.
Bằng cách này, bạn có thể tìm thấy SSDT chứa tất cả 26 Cổng cho bo mạch của mình trong ACPI đã được kết xuất, hãy sửa nó, đặt nó trở lại thư mục `ACPI/patched` và bùng nổ: không cần thêm` USBports.kext` nữa.

## Disable ASPM

Quản lý điện năng ở trạng thái hoạt động (ASPM) là một cơ chế quản lý điện năng dành cho các thiết bị PCI Express để tiết kiệm điện năng trong khi ở trạng thái hoạt động hoàn toàn. Nó thường được sử dụng trên máy tính xách tay và các thiết bị Internet di động khác để kéo dài tuổi thọ pin.

Bản vá này ảnh hưởng đến cài đặt của chính hệ thống ACPI, chẳng hạn như thực tế là quản lý ASPM của Apple không hoạt động như mong đợi. Ví dụ khi sử dụng chipset không phải là bản địa.

## FixHeaders

521 / 5.000
Kết quả dịch
`FixHeaders` sẽ kiểm tra tiêu đề của tất cả các bảng ACPI nói chung, xóa các ký tự Trung Quốc khỏi tiêu đề vì macOS không thể xử lý chúng và gây hoảng loạn ngay lập tức. Sự cố này đã được khắc phục trong macOS Mojave, nhưng trong High Sierra, bạn có thể vẫn cần nó.

Cho dù bạn có gặp sự cố với bảng hay không, bạn có thể bật bản sửa lỗi này một cách an toàn. Nó được khuyến nghị cho tất cả người dùng, ngay cả khi bạn không phải sửa DSDT của mình. Cài đặt cũ bên trong các bản sửa lỗi DSDT vẫn để tương thích ngược nhưng tôi khuyên bạn nên loại trừ nó khỏi phần đó. 

## FixMCFG

453 / 5.000
Kết quả dịch
`MCFG` (Bảng cấu hình được ánh xạ bộ nhớ) mô tả vị trí của không gian cấu hình PCI Express và bảng này sẽ xuất hiện trong quá trình triển khai chương trình cơ sở tuân thủ đặc điểm kỹ thuật này phiên bản 3.0 (hoặc mới hơn). Hữu ích khi sử dụng MacBook và MacBookPro SMBIOS. Tác giả của bản vá là vit9696.

Nếu bật `FixMCFG`, bảng MCFG sẽ được sửa. Tuy nhiên, bạn cũng có thể loại bỏ bảng này với tính năng "Drop Tables". 

## Halt Enabler

Bản vá này là để khắc phục sự cố tắt máy / ngủ trong khi khởi động UEFI. Bản sửa lỗi được đưa vào trước khi gọi `boot.efi`, xóa` SLP_SMI_EN` trước khi khởi động macOS. Tuy nhiên, nó khá an toàn, ít nhất là trên các hệ thống của Intel.

## Patch APIC

Một số hệ thống chỉ có thể được khởi động bằng tham số hạt nhân `cpus = 1` hoặc với một hạt nhân được vá (Lapic NMI). Một phân tích đơn giản cho thấy rằng `MADT` (Bảng mô tả nhiều APIC) của họ bị thiếu phần NMI. Nếu bảng đã hoàn tất, sẽ không có gì thay đổi.

## Reset Address / Reset Value

Hai tham số này phục vụ một mục đích chung - để sửa lỗi khởi động lại. Chúng nên có trong bảng `FADT`, nhưng không phải lúc nào cũng vậy. Đôi khi bảng ngắn hơn mức cần thiết, vì vậy các giá trị này bị thiếu.

Các biến có trong bảng `FACP` nhưng nếu chúng trống, thì` 0x64` / `0xFE` được sử dụng, có nghĩa là khởi động lại qua Bộ điều khiển PS2. Điều này không phải lúc nào cũng hiệu quả với tất cả mọi người. Ngoài ra, bạn có thể sử dụng `0x0CF9 '/` 0x06', điều khiển khởi động lại thông qua Bus PCI. Phương pháp này cũng được sử dụng trên máy Mac gốc, nhưng không phải lúc nào cũng hoạt động trên Hackintoshes. Sự khác biệt rất rõ ràng: trên Hackintoshes cũng có một bộ điều khiển PS2 có thể can thiệp vào việc khởi động lại nếu nó không được đặt lại. Một kết hợp có thể có khác là `0x92 '/` 0x01`.

Cuối cùng nhưng không kém phần quan trọng, bạn có thể đặt chúng thành `0x0 '/` 0x0` để cho phép sử dụng các giá trị `FACP` mặc định. Nếu không có, các trạng thái giá trị mặc định ở trên sẽ được sử dụng thay thế.

## Smart UPS

Tham số này ảnh hưởng đến cấu hình nguồn đã chọn, thông số này sẽ được ghi vào bảng `FADT` và thay đổi nó thành cấu hình` 3`. Logic như sau:

`PM = 1` - Máy tính để bàn, nguồn điện chính </br>
`PM = 2` - Máy tính xách tay, nguồn điện lưới hoặc nguồn pin </br>
`PM = 3` - Máy chủ, được cung cấp bởi SmartUPS, macOS cũng hỗ trợ

Clover sẽ chọn giữa `1` và` 2` dựa trên phân tích bit di động, nhưng cũng có tham số Di động trong phần SMBIOS. Ví dụ, bạn có thể nói rằng chúng tôi có MacMini và nó là thiết bị di động. Giá trị của `3` sẽ được thay thế nếu` smartUPS = Yes`.

## DSDT
### Patches
Trong phần phụ này của `ACPI`, bạn có thể thêm các quy tắc đổi tên (tên nhị phân) để thay thế văn bản bên trong` DSDT` của hệ thống của bạn một cách động bằng mã nhị phân, được biểu thị bằng các giá trị hex. Nói cách khác, bạn thay thế văn bản (ký tự, chữ số và ký hiệu) bằng văn bản khác để tránh xung đột với macOS hoặc để làm cho một số thiết bị / tính năng nhất định hoạt động trong macOS bằng cách đổi tên chúng thành một thứ mà nó biết.

![Bildschirmfoto 2021-05-16 um 07 27 17](https://user-images.githubusercontent.com/76865553/135732656-82fe792e-7225-4255-beb9-d1074eb1522b.png)

Nếu bạn nhìn vào quy tắc đổi tên đầu tiên, `thay đổi EHC1 thành EH01`, nó bao gồm giá trị` Tìm` của `45484331` và giá trị` Thay thế` của `45483031` được dịch theo nghĩa đen là` EHC1` và `EH01` nếu bạn giải mã các giá trị hex trở lại văn bản bằng Trình chuyển đổi Hex trong phần "Công cụ" của Trình cấu hình Clover. Việc đổi tên nào để sử dụng khi nào phụ thuộc vào Bảng Acpi của hệ thống, phiên bản macOS đã sử dụng, v.v. và không phải là một phần của tổng quan này.

### Rename Devices
<details>
<summary><strong>Excursion: Renaming Devices Basics</strong></summary>

**I. About binary renames**

- Các tên thiết bị sau đây được chuẩn hóa để khớp với tên thiết bị được sử dụng trong máy Mac thực và để tạo điều kiện thuận lợi cho việc tạo các bản vá lỗi bộ phận.
- Renaming devices and methods (including [`_DSM`](https://patchwork.kernel.org/project/spi-devel-general/patch/8ce38be1389dfb0527961ccd0dedb609802b59c1.1498044532.git.lukas@wunner.de/)) are applied globally to a system's `DSDT`.


Sau đây là các ví dụ về các thiết bị thường được đổi tên: 

| Thiết bị gốc    | Đã đổi tên cho macOS | Mô tả              |
|:----------------| :--------------------| :------------------|
| EC              | EC0                  | Embedded Controller
| EHC1            | EH01                 | USB Controller
| EHC2            | EH02                 | USB COntroller
| LPC             | LPCB                 | LPC Bus
| SAT1            | SATA                 | SATA Drive
| XHCI            | XHC                  | USB Controller
| Keyboard        | PS2K                 | Keyboard
| System Bus      | SBUS                 | System Management Bus
| Cover           | LID0                 | Lid device (on Laptops)
| PowerButton     | PWRB                 | Power Button Device
| Sleep Button    | SLPB                 | Sleep Button Device

**II. Binary Renames in Detail**

**Prerequisites**: Để áp dụng các tên này một cách chính xác, bạn cần kết xuất `DSDT` của Hệ thống của mình!

Các tên như `_DSM` với và gạch dưới phía trước chúng xác định một phương thức. Chúng thường được đổi tên bằng cách thay thế `_` bằng` X` (như `XDSM`) về cơ bản sẽ vô hiệu hóa phương thức (` XDSM`), vì vậy có thể áp dụng một phương thức khác hoặc phương thức tùy chỉnh.

| # | Đổi tên | Mô tả |
|: -: | : ---------: | -------------------------------- |
| 01 | _DSM sang XDSM | Yêu cầu bản vá khác |
| 02 | LPC thành LPCB | Trong `DSDT`, tìm kiếm` 0x001F0000`. </br> 1: Nếu tên thiết bị đã là `LPCB` thì không cần thay đổi tên. </br> 2: Nếu có nhiều khớp với `0x001F0000`, cẩn thận xác định xem có cần thay đổi tên này hay không?
| 03 | EC đến EC0 | Thay đổi tên của Bộ điều khiển được nhúng. Trong DSDT, hãy kiểm tra thiết bị thuộc về `PNP0C09`. </br> 1: Nếu tên thiết bị đã là` EC0` thì không cần đổi tên </br> 2: Nếu có nhiều kết quả trùng khớp cho `0PNP0C09`, hãy xác nhận real `EC` name </br> 3: Nếu gói ACPI bao gồm` ECDT.aml`, hãy xem "Giới thiệu về ECDT và cách khắc phục" |. |
| 04 | H_EC đến EC0 | Tương tự như EC |
| 05 | ECDV sang EC0 (dell) | Tương tự như EC |
| 06 | EHC1 đến EH01 | Đổi tên Bộ điều khiển USB. Đối với máy có USB2.0, hãy truy vấn tên thiết bị là `0x001D0000`.
| 07 | EHC2 đến EH02 | Đối với Bộ điều khiển USB thứ hai
| 08 | XHCI thành XHC | Thay đổi tên của Bộ điều khiển USB hiện đại. Kiểm tra tên thiết bị thuộc về `0x00140000`. Nếu tên thiết bị đã là `XHC`, không cần đổi tên nhị phân |
| 09 | XHC1 thành XHC | Tương tự như XHCI |
| 10 | KBD thành PS2K | Đổi tên cho Bàn phím. Kiểm tra tên thiết bị của `PNP0303`,` PNP030B`, `PNP0320`. Nếu không xác định được tên bàn phím trong `DSDT`, hãy kiểm tra" tên BIOS "của bàn phím trong Windows Device Manager. Nếu bàn phím đã được đặt tên là `PS2K` thì không cần đổi tên |
| 11 | KBC0 đến PS2K | giống nhau |
| 12 | KBD0 đến PS2K | giống nhau |
| 13 | SMBU thành SBUS | Đổi tên Xe buýt Quản lý Hệ thống. Hầu hết các ThinkPad đều yêu cầu điều này. </br> Trước thế hệ thứ 6, tìm kiếm tên thiết bị thuộc "0x001F0003" </br> máy thế hệ thứ 6 trở lên: tìm kiếm "0x001F0004" thuộc tên thiết bị </br> Nếu tên thiết bị `SBUS` aready, a đổi tên là không cần thiết |
| 14 | LID đến LID0 | Tìm kiếm tên thiết bị thuộc `PNP0C0D` và đổi tên nó thành` LID0`
| 15 | PBTN sang PWRB (dell) | Tìm kiếm thiết bị thuộc về `PNP0C0 ** C **` và đổi tên nó thành `PWRB`
| 16 | SBTN sang SLPB (dell) | Đổi tên Nút Ngủ. Thiết bị tìm kiếm thuộc về `PNP0C0 ** E **`. Nếu tên đã là `SLPB`, không cần đổi tên.

Để chuyển đổi bất kỳ văn bản nào thành hex, bạn có thể sử dụng Hex Converter bên trong Clover Configurator
</details>

`RenameDevices` phục vụ như một phương pháp tinh tế hơn để đổi tên thiết bị ít thô bạo hơn so với việc sử dụng đổi tên nhị phân đơn giản thay thế * mọi * lần xuất hiện của một từ / giá trị trong toàn bộ * toàn bộ *` DSDT`, điều này có thể gây ra vấn đề. Các quy tắc được tạo ở đây chỉ áp dụng cho đường dẫn ACPI được chỉ định trong trường `Tìm thiết bị`, vì vậy các bản vá này chính xác hơn, ít xâm lấn hơn và hiệu quả hơn so với đổi tên chúng qua` DSDT`> `Patches`.

Để sử dụng phần này đúng cách, bạn cần kết xuất `DSDT` chưa sửa đổi và kiểm tra nó bằng maciASL. Trong trường hợp này, chúng tôi tìm kiếm `ECH1`:

![Rename Devices](https://user-images.githubusercontent.com/76865553/135732661-ba636a72-9490-4c6f-a018-bdede3752fa6.jpg)

Như bạn có thể thấy, thiết bị tồn tại và nằm trong `\ SB_PCI0_EHC1` của` DSDT`. Tiếp theo, chúng tôi yêu cầu Trình cấu hình Clover thay thế tên thiết bị thực bằng `EH01` bằng cách thêm nó vào trường` Đổi tên thiết bị`. Sau khi bản vá được áp dụng nhanh chóng trong quá trình khởi động, tên thiết bị và các phụ thuộc của nó đã được thay đổi:

![DSDT_patched](https://user-images.githubusercontent.com/76865553/135732669-9ac77cf7-5c5f-41a0-b98f-0ae1453411dc.png)

### TgtBridge

`TgtBridge` là một trường / chức năng bên trong phần` ACPI> DSDT> Patches` của Clover `config.plist`. Mục đích của nó là giới hạn phạm vi đổi tên nhị phân chỉ hoạt động trong một phần / khu vực được xác định trước của `DSDT`.

Ví dụ: đổi tên phương thức `_STA` thành` _XSTA` trong thiết bị `GPI0`:

![TGTBridgeExample](https://user-images.githubusercontent.com/76865553/135732680-641af3ba-8140-477a-9c9e-69345d8e9b8f.png)

được hiển thị trong ví dụ, tên của Phương thức ban đầu `_STA` (màu hồng) được chuyển đổi thành hex (` 5F535441`), vì vậy Clover có thể tìm thấy nó trong `DSDT`. Nếu nó tìm thấy giá trị này thì nó sẽ được thay thế bằng `58535441` (xanh lam), tương đương với hệ thập lục phân của thuật ngữ` XSTA` (xanh lam). Nếu được đặt như vậy, bản vá này sẽ thay đổi * bất kỳ * sự xuất hiện nào của thuật ngữ `_STA` trong toàn bộ từ` DSDT` thành `XSTA`, điều này có thể sẽ phá vỡ hệ thống. Để tránh điều này, bạn có thể sử dụng `TgtBridge` để chỉ định và giới hạn các kết quả phù hợp của bản vá này với một tên / thiết bị / phương thức / khu vực được chỉ định, trong trường hợp này là thiết bị` GPI0` (Lục lam). Về cơ bản, bạn nói với Clover: "hãy tìm trong` GPI0` và nếu phương thức `_STA` có mặt, hãy đổi tên nó thành` XSTA` nhưng để yên phần còn lại của `DSDT`!"

Giá trị nào để sử dụng trong `TgtBridge` là tùy thuộc vào bạn, nếu bạn biết phải làm gì. Nếu độ dài chuỗi không khớp, Clover sẽ tính đúng cho sự thay đổi độ dài, với một ngoại lệ: đảm bảo điều đó không xảy ra bên trong câu lệnh "If" hoặc "Else". Nếu bạn cần một sự thay đổi như vậy, hãy thay thế toàn bộ nhà điều hành.

Một số giải thích rõ: Trường Nhận xét không chỉ đóng vai trò nhắc nhở nội dung của bản vá mà còn được sử dụng trong Trình đơn khởi động của Clover để bật / tắt các bản vá này. Giá trị ban đầu cho `ON` hoặc` OFF` được xác định bởi các dòng `Disabled` trong` config.plist`. Giá trị mặc định là `Disabled = false`. Nếu bạn sử dụng tập hợp các bản vá lỗi của người khác, tốt hơn nên đặt chúng thành `Disabled = true` và sau đó bật từng bản một từ menu Boot.

**NOTE**: Lỗi TgtBridge (đã sửa kể từ Clover r5123.1)

Trước bản sửa đổi 5123.1, `TgtBridge` đã có một lỗi, nơi nó sẽ không chỉ đổi tên các kết quả trùng khớp được tìm thấy trong DSDT mà còn trong SSDT OEM, điều không dự định xảy ra.

## Fixes [1]

![Bildschirmfoto 2021-05-16 um 07 28 34](https://user-images.githubusercontent.com/76865553/135732689-dd1271db-f11d-468b-a57e-576bcf7f7d76.png)

### AddDTGP

Ngoài `DeviceProperties`, còn có một Device Specific Method (` _DSM`) được chỉ định trong `DSDT` được gọi là` DTGP` để đưa các thông số tùy chỉnh vào một số thiết bị.

`_DSM` là một phương pháp nổi tiếng, được bao gồm trong macOS kể từ phiên bản 10.5. Nó chứa một mảng với mô tả thiết bị và một lệnh gọi đến phương thức `DTGP` chung, phương thức này giống nhau đối với tất cả các thiết bị. Nếu không có phương thức `DTGP`,` DSDTs` đã sửa đổi sẽ không hoạt động tốt. Bản sửa lỗi này chỉ cần thêm phương pháp này để sau đó nó có thể được áp dụng cho các bản sửa lỗi khác. Nó không tự hoạt động một mình.

### AddMCHC

Thêm thiết bị MCHC có liên quan đến Bộ điều khiển bộ nhớ và được kết hợp hoàn toàn với `FixSBUS` để Bộ điều khiển Bus quản lý hệ thống hoạt động. Nói chung, thiết bị thuộc lớp `0x060000` không có trong DSDT, nhưng đối với một số chipset, thiết bị này có thể sử dụng được và do đó nó phải được gắn vào I / O Reg để đấu dây đúng cách cho việc quản lý nguồn của bus PCI.

### FixAirport

Tương tự như mạng LAN, bản thân thiết bị được tạo, nếu chưa được đăng ký trong `DSDT`. Đối với một số kiểu máy nổi tiếng, `DeviceID` được thay thế bằng một ID được hỗ trợ. Và Sân bay bật mà không có các bản vá lỗi khác.

### FixDarwin

Bắt chước Windows XP trong Hệ điều hành Darwin. Nhiều vấn đề về giấc ngủ và độ sáng bắt nguồn từ việc xác định sai hệ thống.

### FixDisplay

Sản xuất một số bản vá lỗi thẻ video cho các thẻ video không phải của Intel (đồ họa trên bo mạch của Intel yêu cầu các bản sửa lỗi khác nhau):

- Chích các thuộc tính và chính các thiết bị (nếu không có).
- Có thể tiêm FakeID. Nếu một tham số FakeID được chỉ định, thì nó sẽ được đưa vào thông qua phương thức `_DSM`.
- Thêm thuộc tính tùy chỉnh.
- Thêm thiết bị `HDAU` để xuất âm thanh qua HDMI.

### FixFirewire

Thêm thuộc tính "fwhub" vào bộ điều khiển Firewire, nếu có. Nếu không, thì sẽ không có gì xảy ra. Bạn có thể đặt cược nếu bạn không biết mình có cần hay không.

### FixHDA

Chỉnh sửa mô tả của card âm thanh trong `DSDT` để trình điều khiển AppleHDA gốc hoạt động. Đổi tên `AZAL` thành` HDEF` được thực hiện, `layout-id` và` PinConfiguration` được đưa vào. Ngày nay đã lỗi thời vì `AppleALC.kext` xử lý điều này (kết hợp với` layout-id` chính xác được nhập trong phần `Thiết bị`).

### FixHPET

Như đã đề cập, đây là bản sửa lỗi chính cần thiết. Do đó, mặt nạ vá `DSDT` bắt buộc tối thiểu trông giống như` 0x0010`.

### FixIDE

Trong macOS 10.6.1, có một sự cố trên `AppleIntelPIIXATA.kext`. Hai giải pháp cho vấn đề được biết đến: sử dụng kext đã sửa hoặc sửa thiết bị trong `DSDT`. Và đối với các hệ thống hiện đại hơn? Sử dụng nó, nếu có một bộ điều khiển như vậy (rất khó xảy ra vì IDE đã lỗi thời).

### FixIPIC

Loại bỏ ngắt khỏi thiết bị `IPIC`. Sửa chức năng Nút nguồn, vì vậy, giữ nút này trong vài giây sẽ mở ra cửa sổ hộp thoại với tùy chọn Đặt lại, Ngủ hoặc Tắt máy tính.

### FixLAN
Việc bổ sung thuộc tính "tích hợp sẵn" cho card mạng là cần thiết để hoạt động chính xác. Cũng là một mô hình thẻ được tiêm - cho mỹ phẩm.

### FixSATA

Khắc phục một số sự cố với SATA và loại bỏ màu vàng của các biểu tượng đĩa trong hệ thống bằng cách bắt chước theo ICH6. Trên thực tế, một phương pháp gây tranh cãi, tuy nhiên, nếu không có bản sửa lỗi này, các đĩa DVD của tôi sẽ không phát và đối với đĩa DVD, ổ đĩa không thể tháo rời. Những thứ kia. chỉ thay thế biểu tượng không phải là một tùy chọn! Có một giải pháp thay thế, được giải quyết bằng cách thêm bản sửa lỗi với `AppleAHCIport.kext`. Xem chương về vá kexts. Và, theo đó, bit này có thể được bỏ qua! Một trong số ít bit tôi khuyên bạn không nên sử dụng.

### FixSBus

Thêm Bộ điều khiển Bus quản lý hệ thống vào cây thiết bị, do đó loại bỏ cảnh báo về sự vắng mặt của nó khỏi nhật ký hệ thống. Nó cũng tạo ra bố trí quản lý điện năng xe buýt chính xác, ảnh hưởng đến giấc ngủ. Để kiểm tra xem SBUS có hoạt động chính xác hay không, hãy nhập `kextstat | grep -E "AppleSMBusController | AppleSMBusPCI" `trong thiết bị đầu cuối. Nếu đầu ra Terminal chứa 2 trình điều khiển sau thì SMBus của bạn đang hoạt động chính xác: `com.apple.driver.AppleSMBusPCI` và` com.apple.driver.AppleSMBusController`
### FixShutdown

Một điều kiện được thêm vào phương thức `_PTS` (chuẩn bị ngủ): nếu đối số = 5 (tắt máy), thì không cần thực hiện hành động nào khác. Kỳ lạ, tại sao? Tuy nhiên, đã có nhiều lần xác nhận về tính hiệu quả của bản vá này đối với bo mạch của ASUS, có thể đối với những người khác. Một số `DSDT` đã có một kiểm tra như vậy, trong trường hợp đó, một bản sửa lỗi như vậy sẽ bị vô hiệu hóa. Nếu `SuspendOverride` =` true` được đặt trong cấu hình, thì bản sửa lỗi này sẽ được mở rộng bởi các đối số 3 và 4. Tức là chuyển sang chế độ ngủ (Suspend). Mặt khác, nếu `HaltEnabler` =` true`, thì bản vá này có thể không còn cần thiết nữa.

### FixUSB

Cố gắng giải quyết nhiều vấn đề về USB. Đối với bộ điều khiển `XHCI`, khi sử dụng bản vá` IOUSBFamily.kext`, một bản vá `DSDT` như vậy là không thể thiếu. Trình điều khiển của Apple đặc biệt sử dụng ACPI và DSDT phải được viết đúng chính tả. Điều này đảm bảo rằng không có xung đột với các chuỗi trong `DSDT`.

## Fixes [2]

![Bildschirmfoto 2021-05-16 um 08 04 15](https://user-images.githubusercontent.com/76865553/135732698-6ead0af4-304c-4570-a407-aaafb70506f2.png)

### AddHDMI
Thêm thiết bị âm thanh `HDAU` vào` DSDT` phù hợp với đầu ra HDMI của card màn hình ATI và Nvidia để cung cấp âm thanh qua HDMI. Vì GPU được mua riêng biệt với bo mạch chủ, đơn giản là không có thiết bị nào như vậy trong DSDT gốc. Ngoài ra, thuộc tính `hda-gfx = onboard-1` hoặc` onboard-2` được đưa vào thiết bị:

* `1` nếu` UseIntelHDMI` = false
* `2` nếu có cổng Intel chiếm cổng 1.

### AddIMEI
Thêm thiết bị Intel Management Engine (IMEI) vào cây thiết bị, nếu nó không tồn tại trong `DSDT`. IMEI là bắt buộc để giải mã video phần cứng thích hợp trên Intel iGPUs. Thêm IMEI chỉ được yêu cầu trong hai trường hợp:

- CPU Sandy Bridge chạy trên bo mạch chủ 7-series hoặc
- CPU Ivy Bridge chạy trên bo mạch chủ 6-series

### ThêmPNLF

Chèn thiết bị PNLF (Đèn nền), thiết bị này cần thiết để kiểm soát độ sáng màn hình một cách hợp lý và kỳ lạ thay, giúp giải quyết vấn đề khi ngủ, kể cả đối với máy tính để bàn.

### PNLF_UID

Có một số đường cong / đồ thị độ sáng mẫu trong hệ thống và chúng có các UID khác nhau. Nếu một số nhà môi giới sử dụng đường cong đó, điều đó không có nghĩa là bạn sẽ có cùng độ sáng với cùng một bộ xử lý. Nó phụ thuộc vào bảng điều khiển - không phải bộ xử lý. Nói chung, nên sử dụng `SSDT-PNLF.aml`. Bạn có thể tìm thấy một trong Thư mục Mẫu của Gói OpenCore.

### DeleteUnused

Loại bỏ các thiết bị đĩa mềm, CRT và DVI không sử dụng - điều cần thiết để Intel GMA X3100 hoạt động trên Máy tính xách tay Dell. Nếu không, bạn sẽ nhận được một màn hình đen. Đã được kiểm tra bởi hàng trăm người dùng.

### FakeLPC

Thay thế DeviceID của bộ điều khiển LPC để kext AppleLPC được gắn vào nó. Nó cần thiết cho những trường hợp khi chipset không được cung cấp cho macOS (ví dụ ICH9). Tuy nhiên, danh sách các chipset Intel và nForce nguyên bản quá lớn nên nhu cầu về một bản vá như vậy là rất hiếm. Nó kiểm tra trong hệ thống xem kext AppleLPC đã được tải chưa và nếu không, bản vá là cần thiết.

### FixACST

Một số DSDT có thể có thiết bị, phương thức hoặc biến có tên là `ACST`, nhưng tên này cũng được macOS 10.8+ sử dụng để điều khiển C-States!

Kết quả là, một xung đột hoàn toàn tiềm ẩn với hành vi rất không rõ ràng có thể xảy ra. Bản sửa lỗi này đổi tên tất cả các lần xuất hiện của `ACST` thành` OCST` là an toàn. Nhưng hãy kiểm tra DSDT của bạn trước: tìm kiếm `ACST` và kiểm tra xem nó có tham chiếu đến Thiết bị` AC` và Phương thức `_PSR` (_PSR: PowerSource) theo một cách nào đó hay không.

### FixADP1

Sửa thiết bị `ADP1` (nguồn điện), thiết bị cần thiết để máy tính xách tay ngủ chính xác - đã cắm hoặc rút phích cắm.

### FixDarwin7

Tương tự như `FixDarwin`, nhưng dành cho Windows 7. Các` DSDT` cũ có thể không có kiểm tra hệ thống như vậy. Bây giờ bạn có tùy chọn.

### FixIntelGfx

Bản vá cho đồ họa tích hợp của Intel được tách biệt với phần còn lại của các cạc đồ họa, tức là bạn có thể đặt bản vá cho Intel và không đặt cho Nvidia.

### FixMutex

Bản vá này tìm tất cả các đối tượng Mutex và thay thế `SyncLevel` bằng` 0`. Chúng tôi sử dụng bản vá này vì macOS không hỗ trợ gỡ lỗi Mutex thích hợp và sẽ bị lỗi khi có bất kỳ yêu cầu nào với Mutex có SyncLevel nonzero. Các đối tượng Nonzero SyncLevel Mutex là một trong những nguyên nhân phổ biến gây ra lỗi phương pháp pin ACPI. Được Rehabman thêm vào trong các bản sửa đổi r4265 đến r4346.

Ví dụ, trong Lenovo u430 mutexes được khai báo như thế này:

`Mutex (MSMI, 0x07) '

Để làm cho nó tương thích với macOS, bạn cần thay đổi nó thành:

`Mutex (MSMI, 0) '

Đây là một bản vá gây tranh cãi rất nhiều. Chỉ sử dụng nó nếu bạn hoàn toàn nhận thức được những gì bạn đang làm.

### FixRegions

Đây là một bản vá rất đặc biệt. Trong khi các bản vá khác trong phần này được thiết kế để sửa lỗi `BIOS.aml` nhằm tạo một DSDT tùy chỉnh tốt từ đầu, thì bản sửa lỗi này được thiết kế để điều chỉnh một` DSDT.aml` tùy chỉnh hiện có.

DSDT có các vùng có địa chỉ riêng của chúng, chẳng hạn như:
`OperationRegion` (GNVS, SystemMemory, 0xDE6A5E18, 0x01CD). Vấn đề là địa chỉ vùng này được tạo * động * bởi BIOS và nó có thể khác từ khởi động đến khởi động. Điều này lần đầu tiên được nhận thấy khi thay đổi tổng dung lượng bộ nhớ, sau đó khi thay đổi cài đặt BIOS và trên máy tính của tôi, nó thậm chí còn phụ thuộc vào lịch sử trước khi khởi động, chẳng hạn như số lượng NVRAM bị chiếm dụng. Rõ ràng, trong `DSDT.aml` tùy chỉnh, con số này là cố định và do đó có thể không đúng. Quan sát đơn giản nhất là tình trạng thiếu ngủ. Sau khi cố định một vùng, chế độ ngủ sẽ xuất hiện, nhưng chỉ cho đến lần bù đắp tiếp theo. Bản sửa lỗi này sửa tất cả các vùng trong DSDT tùy chỉnh thành các giá trị trong BIOS DSDT, và do đó, mặt nạ

```swift 
<key>Fixes</key>
<dict>
	<key>FixRegions</key>
	<true/>
</dict>
```
là đủ nếu bạn có một `DSDT` tùy chỉnh được thực hiện tốt với tất cả các bản sửa lỗi. Có một bản vá khác, nhưng nó không dành riêng cho DSDT mà dành cho tất cả các bảng ACPI nói chung, vì vậy việc thêm nó vào Phần ACPI là không phù hợp.

### FixRTC
Loại bỏ ngắt khỏi thiết bị `_RTC`. Đó là một bản sửa lỗi bắt buộc và rất lạ là ai đó không kích hoạt nó. Nếu không có gián đoạn trong bản gốc, thì bản vá này sẽ không gây ra bất kỳ tác hại nào. Tuy nhiên, câu hỏi nảy sinh về sự cần thiết phải chỉnh sửa độ dài của khu vực. Để tránh xóa `CMOS`, bạn cần đặt độ dài thành` 2`, nhưng đồng thời một cụm từ như `"… only single bank… "` xuất hiện trong Nhật ký hạt nhân.

Tôi không biết có vấn đề gì với thông báo này, nhưng nó có thể được loại trừ nếu độ dài được đặt thành 8 byte bằng cách sử dụng Fix `Rtc8Allowed`:

```swift 
<key>ACPI</key>
<dict>
	<key>DSDT</key>
	<dict>
	<key>Rtc8Allowed</key>
	<false/>
</dict>
```

* `true` - độ dài của vùng sẽ vẫn là 8 byte, nếu có,
* `false` - sẽ được sửa bằng 2 byte, điều này ngăn CMOS được đặt lại một cách đáng tin cậy hơn.

Theo nghiên cứu của vit9696, độ dài vùng vẫn phải là `8 ', vì bạn cần nó để lưu khóa ngủ đông. Vì vậy, bản thân bản sửa lỗi rất hữu ích. Vì không cần chế độ ngủ đông trên Máy tính để bàn, bạn có thể cân nhắc đặt lại CMOS.

### FixS3D

Tương tự như vậy, bản vá này giải quyết vấn đề với giấc ngủ.

### FixTMR

Loại bỏ ngắt khỏi bộ định thời _TMR theo cách tương tự. Nó không còn được sử dụng trên các máy Mac mới hơn nhưng trên Ivy Bridge, nó vẫn được yêu cầu giải quyết xung đột IRQ để âm thanh hoạt động (kết hợp với `FixHPET`,` FixIPIC` và `FixRTC`)

### FixWAK

Thêm Quay lại phương thức `_WAK`. Nó phải như vậy, nhưng vì một số lý do thường `DSDT` không chứa nó. Rõ ràng các tác giả đã tuân thủ một số tiêu chuẩn khác. Trong mọi trường hợp, bản sửa lỗi này là hoàn toàn an toàn.

## SSDT
![SSDT](https://user-images.githubusercontent.com/76865553/136655891-edd9c38d-852f-476e-9ca7-4295cdb4ec38.png)

Phần này dành cho việc bật / sửa / tối ưu hóa Quản lý nguồn CPU. Nó hiếm khi được sử dụng ngày nay, vì các công cụ như [** ssdtPRGen **](https://github.com/Piker-Alpha/ssdtPRGen.sh) or [**SSDTTime**](https://github.com/corpnewt/SSDTTime) can generate a dedicated `SSDT` for it instead.

### C3 Latency

Giá trị này xuất hiện trong máy Mac thực, đối với iMac là khoảng 200, đối với MacPro là khoảng 10. Theo tôi, iMac được điều chỉnh bởi P-stats, MacPros được điều chỉnh bởi C-stats. Và nó cũng phụ thuộc vào chipset, liệu nó có đáp ứng đầy đủ các lệnh trạng thái D từ MacOS hay không. Tùy chọn an toàn và dễ dàng nhất là *** không đặt tham số này *** và mọi thứ sẽ hoạt động tốt như hiện tại.

### Double First State

Để [Speedstep] (https://en.wikipedia.org/wiki/SpeedStep) hoạt động chính xác, cần phải sao chép trạng thái đầu tiên của bảng P-state. Mặc dù sự cần thiết của bản sửa lỗi này đã trở nên nghi ngờ đối với các CPU mới hơn, nhưng nó vẫn có liên quan khi sử dụng các CPU Intel thế hệ thứ 3 (Tên mã là `IvyBridge`).

### Drop OEM

Vì chúng ta sẽ tự động tải các bảng SSDT của riêng mình, nên chúng ta cần tránh các mã chồng chéo không cần thiết để tránh xung đột. Tùy chọn này cho phép bạn loại bỏ tất cả các bảng gốc để chuyển sang các bảng mới.

Nếu bạn muốn tránh hoàn toàn việc vá các bảng SSDT, có một tùy chọn khác: đặt các bảng gốc có các chỉnh sửa nhỏ trong Thư mục `EFI / OEM / xxx / ACPI / p patch /` và hủy các bảng chưa được vá. Tuy nhiên, nên sử dụng phương pháp Drop có chọn lọc đã đề cập ở trên.

### Enable C2, C4, C6 and C7

Chỉ định C-States bạn muốn bật / tạo.

### Generate
![Generate_Options](https://user-images.githubusercontent.com/76865553/136794188-a2fac646-48bb-469d-87f9-ca47afe00622.png)

Trong Clover mới, nhóm tham số này được kết hợp thành một phần và `PluginType` giờ chỉ là` true` hoặc `false`. Khóa `PluginType` khác (khóa có menu thả xuống) đã lỗi thời và chỉ được giữ lại để tương thích ngược!

Các tham số APLF và APSN dường như ảnh hưởng đến tốc độ, nhưng đối với những người biết chúng dùng để làm gì. Lưu ý: Vì APSN / APLF là một phần của Tạo → PStates, chúng hoạt động nếu Tạo → PStates = true, trong khi PluginType độc ​​lập và hoạt động độc lập với lựa chọn Tạo → PStates.

#### APLF

#### APSN

**NOTE**: Vì APSN / APLF là các thành phần của `PStates`, chúng * chỉ * hoạt động * nếu *` PStates` được bật (`true`), trong khi` PluginType` hoạt động độc lập với `PStates`.

**IMPORTANT**:  Không cần tùy chọn `Tạo 'nếu SSDT-PM tùy chỉnh đã được tạo bằng ssdtPRGen hoặc SSDTTime!

#### CStates/PStates

Ở đây chúng tôi chỉ định rằng hai bảng bổ sung được tạo cho C-States và P-States, theo các quy tắc do cộng đồng hackintosh phát triển. Đối với C-States, bảng có các thông số C2, C4, C6, Latency được đề cập trong phần CPU. Cũng có thể chỉ định những cái trong phần SSDT.

#### PluginType

Đối với các CPU Haswell và mới hơn, bạn nên đặt khóa thành `1`, đối với các CPU cũ hơn là` 0`. Khóa này, cùng với khóa Generate → `PluginType`, giúp bạn có thể tạo bảng SSDT chỉ chứa` PluginType`, nhưng không có P-States nếu quá trình tạo của chúng bị vô hiệu hóa. Chìa khóa này không cần thiết; nó đã được lưu để tương thích ngược.

### Min Multiplier

Số nhân CPU tối thiểu. Bản thân nó báo cáo 16 và thích chạy ở 1600, nhưng bạn nên đặt số liệu thống kê xuống 800 hoặc thậm chí 700 trong bảng để có tốc độ. Thử nghiệm với nó. Nếu hệ thống của bạn gặp sự cố trong khi khởi động, thì Tần suất thấp quá thấp!

### Max Multiplier

Được giới thiệu cùng với Min Multiplier, nhưng nó dường như không có tác dụng gì và không nên sử dụng. Tuy nhiên, bằng cách nào đó, nó ảnh hưởng đến số lượng trạng thái P, vì vậy bạn có thể thử nghiệm với nó, nhưng bạn không nên làm điều đó nếu không có nhu cầu đặc biệt.

### NoDynamicExtract

Nếu được đặt thành `true`, cờ này sẽ vô hiệu hóa việc trích xuất các SSDT động khi sử dụng` F4` trong menu bootloader. Các SSDT động hiếm khi cần thiết và thường gây nhầm lẫn (đặt nhầm chúng vào Thư mục `ACPI / đã vá lỗi`). Được Rehabman thêm vào trong bản sửa đổi 4359.

### NoOEMTableID

Nếu được đặt thành `true`, mã định danh bảng OEM là * KHÔNG * được thêm vào cuối tên tệp trong kết xuất bảng ACPI bằng cách nhấn` F4` trong Trình đơn khởi động của Clover. Nếu được đặt thành `false`, dấu cách kết thúc sẽ bị xóa khỏi tên SSDT khi ID bảng OEM được thêm làm hậu tố. Được Rehabman thêm vào trong các bản sửa đổi 4265 đến 4346.

### PLimit Dict

`PLimitDict` giới hạn tốc độ tối đa của bộ xử lý. Nếu được đặt thành `0` - tốc độ là tối đa,` 1` - dưới mức tối đa một bậc. Nếu thiếu phím này ở đây, bộ xử lý sẽ bị kẹt ở tần số tối thiểu.

### Use SystemIO

Nếu được đặt thành `true`, phần SSDT sẽ được sử dụng để chọn trong bảng` _CST` được tạo giữa:

`` nhanh chóng
Đăng ký (FFixedHW,
Đăng ký (SystemIO,
``
### UnderVolt Step

Tham số tùy chọn để giảm nhiệt độ CPU bằng cách giảm điện áp hoạt động của nó. Các giá trị có thể là 0 đến 9. Giá trị càng cao, điện áp càng giảm, dẫn đến nhiệt độ thấp hơn - cho đến khi máy tính bị treo. Đây là lúc bảo vệ tuyệt đối xuất hiện: Clover sẽ không cho phép bạn đặt bất kỳ giá trị nào ngoài phạm vi được chỉ định. Tuy nhiên, ngay cả các giá trị cho phép cũng có thể dẫn đến hoạt động không ổn định. Hiệu ứng của undervolting thực sự đáng chú ý. Tuy nhiên, thông số này chỉ áp dụng cho các CPU Intel thuộc họ `Penryn`.

## Drop Tables

![Bildschirmfoto 2021-05-16 um 08 28 35](https://user-images.githubusercontent.com/76865553/135732583-c8d61605-03af-4b78-a4db-4df9d1e68d56.png)

Trong mảng này, bạn có thể liệt kê các bảng sẽ bị loại bỏ khi tải. Chúng bao gồm các chữ ký bảng khác nhau, chẳng hạn như `DMAR`, thường bị loại bỏ vì macOS không thích công nghệ` VT-d`. Các bảng khác sẽ bị loại bỏ sẽ là `MATS` (khắc phục sự cố với High Sierra) hoặc` MCFG` bởi vì bằng cách chỉ định kiểu MacBookPro hoặc MacMini, chúng tôi nhận được sự cố nghiêm trọng. Một phương pháp tốt hơn đã được phát triển (xem phần `` FixMCFG`)

## DisabledAML

![Bildschirmfoto 2021-05-16 um 08 31 09](https://user-images.githubusercontent.com/76865553/135732710-df439b95-b7b9-4e88-bd47-0bc082ec63a6.png)

Bạn có thể thêm SSDT từ thư mục `ACPI / p patch` sẽ được bỏ qua khi tải khi khởi động hệ thống.

## Sorted Order

Tạo một mảng để tải SSDT trong thư mục `ACPI / p patch` theo thứ tự được chỉ định trong danh sách này sau khi bạn thêm SSDT vào danh sách này. Chỉ các SSDT có trong mảng này mới được tải, cụ thể là theo thứ tự được chỉ định.

Nói chung, một vấn đề với các bảng là tên của chúng. Mặc dù không có gì lạ khi OEM Tabkes sử dụng bảng chữ cái quốc gia, hoặc không có tên, đối với Apple, điều đó là không thể chấp nhận được. Tên phải có 4 ký tự trong bảng chữ cái La Mã. Sử dụng "FixHeaders" để khắc phục sự cố này.

### Debug

![Debug](https://user-images.githubusercontent.com/76865553/136655999-a2c369e4-14d3-4410-87ad-7473da46c749.png)

Bật Nhật ký gỡ lỗi sẽ được lưu trữ trong `EFI / CLOVER / misc / debug.log`. Việc kích hoạt tính năng này sẽ làm chậm quá trình khởi động đáng kể nhưng giúp giải quyết các vấn đề.

### RTC8Allowed
See "Fixes [2]" Section → "[**FixRTC**](https://github.com/5T33Z0/Clover-Crate/tree/main/ACPI#fixrtc)".

### ReuseFFFF
Trong một số trường hợp, nỗ lực vá lỗi GPU bị cản trở bởi sự hiện diện của:

```swift 
Device (PEGP) type of device
	{
	Name (_ADR, 0xFFFFFF)
	Name (_SUN, One)
	}
```
Bạn có thể thay đổi địa chỉ của nó thành `0`, nhưng điều đó không phải lúc nào cũng hiệu quả. ** LƯU Ý **: Bản sửa lỗi này không được dùng nữa. Bản sửa lỗi này đã bị xóa khỏi Clover kể từ r5116!

### SlpSmiAtWake

Thêm `SLP_SMI_EN = 0` vào mỗi lần thức dậy. Điều này có thể giúp giải quyết các vấn đề về chế độ ngủ và tắt máy khi khởi động UEFI. ** LƯU Ý **: Bản sửa lỗi này không được dùng nữa và đã bị xóa khỏi Clover kể từ r5134!

### SuspendOverride

Bản vá tắt máy chỉ hoạt động ở trạng thái nguồn `5 '(tắt máy). Tuy nhiên, chúng tôi có thể muốn mở rộng bản vá này sang trạng thái `3` và` 4` bằng cách bật `SuspendOverride`.

Điều này hữu ích khi chuyển sang chế độ ngủ trong khi khởi động UEFI. Hiện tượng: màn hình sẽ tắt nhưng đèn và quạt vẫn tiếp tục chạy.
Tin tặc nâng cao có thể sử dụng đổi tên nhị phân để sửa nó (không được đề cập ở đây).

### DSDT name:
Tại đây, bạn có thể chỉ định tên của DSDT ** đã được vá ** tùy chỉnh nếu nó được gọi là thứ gì đó khác với `DSDT.aml`, để Clover chọn và áp dụng nó.