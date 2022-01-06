# Boot
![Boot](https://user-images.githubusercontent.com/76865553/136703418-a28fed86-1f46-4519-80ad-671e96b89141.jpeg)
### Arguments

Đây là các đối số khởi động được chuyển tới `boot.efi`, sau đó chuyển chúng xuống hạt nhân hệ thống. Xem tài liệu của Apple để biết danh sách trong hướng dẫn sử dụng `com.apple.Boot.plist`. Một số cái thường được sử dụng là:

#### Debugging
|Boot-arg | Mô tả |
|: ------: | ----------- |
** `-v` ** | Chế độ _V_erbose. Thay thế thanh tiến trình bằng đầu ra đầu cuối với một nhật ký khởi động giúp giải quyết các sự cố. Kết hợp với `debug = 0x100` và` keepyms = 1`
** `-f` ** | _F_orce-xây dựng lại bộ nhớ cache kext khi khởi động.
** `-s` ** | Chế độ người dùng _S_ingle. Thao tác này sẽ khởi động macOS ở chế độ dựa trên thiết bị đầu cuối, có thể được sử dụng để sửa chữa hệ thống của bạn. Nếu bạn lo lắng về bảo mật, hãy đặt quirk `DisableSingleUser` thành true vì Chế độ một người dùng có thể được sử dụng để bỏ qua mật khẩu tài khoản Quản trị viên.
** `-x` ** | Chế độ An toàn. Khởi động macOS với một tập hợp tối thiểu các tính năng và tiện ích mở rộng hệ thống. Nó cũng có thể kiểm tra đĩa khởi động của bạn để tìm và sửa các lỗi như chạy Sơ cứu trong Disk Utility. Có thể được kích hoạt từ OC bootmenu bằng cách giữ tổ hợp phím nếu bật `PollAppleHotkeys`.
** `debug = 0x100` ** | Tắt cơ quan giám sát của macOS. Ngăn không cho máy khởi động lại khi bị nhiễu hạt nhân. Bằng cách đó, bạn hy vọng có thể thu thập một số thông tin hữu ích và làm theo đường dẫn để vượt qua sự cố.
** `keepyms = 1` ** | Cờ đồng hành thành` debug = 0x100` cho biết hệ điều hành cũng in các ký hiệu trên một hạt nhân hoảng loạn. Có thể cung cấp một số thông tin chi tiết hữu ích về nguyên nhân gây ra cơn hoảng loạn.
** `dart = 0` ** | Tắt VT-x / VT-d. Ngày nay, `DisableIOMapper` Quirk được sử dụng để thay thế.
** `cpus = 1` ** | Giới hạn số lõi CPU là 1. Hữu ích trong trường hợp macOS không khởi động hoặc cài đặt theo cách khác.
** `npci = 0x2000` ** / **` npci = 0x3000` ** | Tắt gỡ lỗi PCI liên quan đến `kIOPCIConfiguratorPFM64`. Ngoài ra, hãy sử dụng `npci = 0x3000` để tắt gỡ lỗi của` gIOPCITunnelledKey`. Bắt buộc khi bị kẹt ở `Cấu hình khởi động PCI` vì có xung đột IRQ liên quan đến các làn PCI của bạn. ** Không cần thiết nếu có thể bật `Trên4GDecoding` trong BIOS **
** `-no_compat_check` ** | Tắt kiểm tra tính tương thích của macOS. Ví dụ: macOS 11.0 BigSur không còn hỗ trợ các mẫu iMac được giới thiệu trước năm 2014. Việc bật điều này cho phép cài đặt và khởi động macOS trên SMBIOS không được hỗ trợ. Nhược điểm: bạn không thể cài đặt các bản cập nhật hệ thống nếu tính năng này được bật.

#### GPU-specific boot arguments
Để biết thêm các args khởi động liên quan đến iGPU và dGPU, hãy xem chủ đề Dù xanh.

| Boot-arg | Mô tả |
|: ------: | ----------- |
**`agdpmod = pikera`**|Tắt kiểm tra Board-ID trên GPU AMD Navi (dòng RX 5000 & 6000). Nếu không có điều này, bạn sẽ nhận được một màn hình đen. Không sử dụng trên GPU Polaris và Vega.
**`-igfxvesa`** | Tắt tăng tốc đồ họa Intel để hỗ trợ kết xuất phần mềm. Hữu ích nếu đồ họa tích hợp không tương thích với macOS.
**`-wegnoegpu`** | Tắt tất cả GPU trừ đồ họa tích hợp trên CPU Intel. Sử dụng nếu GPU không tương thích với macOS. Không hoạt động mọi lúc.
**`nvda_drv = 1`** | Bật Trình điều khiển web cho Thẻ đồ họa NVIDIA lên đến macOS 10.11. Từ macOS 10.12 trở đi, thay vào đó hãy sử dụng tính năng `NvidiaWeb` trong phần Thông số hệ thống được ghi vào NVRAM.
** `nv_disable = 1` ** | Tắt tăng tốc phần cứng của GPU NVIDIA (*** không *** kết hợp điều này với` nvda_drv = 1`). Sử dụng điều này nếu màn hình của bạn tắt khi cài đặt macOS. Nó bật chế độ Vesa để bạn có được một bức ảnh. Sau khi cài đặt Nvidia WebDrivers (chỉ hỗ trợ lên đến High Sierra), bạn có thể tắt boot-arg này một lần nữa.

#### Network-specific boot arguments
|Boot-arg | Mô tả |
|: ------: | ----------- |
** `dk.e1000 = 0` ** | Cấm` com.apple.DriverKit-AppleEthernetE1000` (trình điều khiển DEXT của Apple) gắn vào bộ điều khiển Intel I225-V Ethernet được sử dụng trên bo mạch Comet Lake cao cấp hơn, khiến trình điều khiển kext I225 của Apple để tải thay thế. Đối số khởi động này là tùy chọn trên hầu hết các bo mạch vì chúng tương thích với trình điều khiển DEXT. Tuy nhiên, nó có thể được yêu cầu trên Gigabyte và một số bo mạch khác, chỉ có thể sử dụng trình điều khiển kext, vì trình điều khiển DEXT gây ra treo. Bạn không cần điều này nếu bo mạch của bạn không xuất xưởng với I225-V NIC.

#### Other useful boot arguments
| Boot-arg | Mô tả |
|: ------: | ----------- |
** `alcid = 1` ** | Để chọn layout-id cho AppleALC, trong khi giá trị số chỉ định layout-id. Xem [codec được hỗ trợ](https://github.com/acidanthera/applealc/wiki/supported-codecs) để tìm ra bố cục nào sẽ sử dụng cho hệ thống cụ thể của bạn.
** `amfi_get_out_of_my_way = 1` ** | Kết hợp với SIP bị tắt, điều này sẽ vô hiệu hóa tính toàn vẹn của tệp di động Apple. AMFI là một mô-đun hạt nhân macOS thực thi xác thực ký mã và xác thực thư viện để tăng cường bảo mật. Ngay cả sau khi vô hiệu hóa các dịch vụ này, AMFI vẫn kiểm tra chữ ký của mọi ứng dụng đang chạy và sẽ khiến các ứng dụng không phải của Apple gặp sự cố khi chúng chạm vào các khu vực cực nhạy của hệ thống. Ngoài ra còn có một [kext](https://github.com/osy/AMFIExemption) thực hiện điều này trên cơ sở mỗi ứng dụng

#### Provided by Lilu.kext
ACác loại giày bốt Lilu. Hãy nhớ rằng Lilu là một Patch engine cung cấp chức năng cho hầu hết (?) Bất kỳ kext nào khác trong vũ trụ hackintosh, vì vậy bạn phải lưu ý điều đó nếu sử dụng bất kỳ lệnh nào trong số này!

| Boot-arg | Mô tả |
|: -------: | ----------- |
`-liluoff` | Vô hiệu hóa Lilu.
`-lilubeta` | Bật Lilu trên các phiên bản macOS không được hỗ trợ (macOS 12 trở xuống được hỗ trợ theo mặc định).
`-lilubetaall` | Bật Lilu và * tất cả * các plugin đã tải trên macOS không được hỗ trợ (sử dụng _very_ cẩn thận).
`-liluforce` | Cho phép Lilu bất kể chế độ, hệ điều hành, trình cài đặt hoặc khôi phục.
`liludelay = 1000` | Thêm độ trễ 1 giây (1000 mili giây) sau mỗi lần in để khắc phục sự cố.
`lilucpu = N` | để cho Lilu và các plugin giả sử thứ N CPUInfo :: CpuGeneration.

#### Provided by Whatevergreen.kext 
Được liệt kê bên dưới, bạn sẽ tìm thấy một số lượng khởi động ngắn gọn nhưng hữu ích của Anythinggreen cho mọi thứ liên quan đến đồ họa. Kiểm tra [danh sách đầy đủ] (https://github.com/acidanthera/W AnythingGreen/blob/master/README.md#boot-arguments) để tìm nhiều hơn nữa.

| Boot-arg | Mô tả |
|: ------: | ----------- |
`-wegoff` | Vô hiệu hóa AnythingGreen.
`-wegbeta` | Bật tùy chọn AnythingGreen trên các phiên bản hệ điều hành không được hỗ trợ.
`-wegswitchgpu` | Vô hiệu hóa iGPU thứ nếu phát hiện thấy một GPU rời (hoặc sử dụng thuộc tính` switch-to-external-gpu` cho iGPU)
`-wegnoegpu` | Tắt tất cả các GPU rời (hoặc thêm thuộc tính` disable-gpu` vào mỗi GFX0).
`-wegnoigpu` | Tắt GPU bên trong (hoặc thêm thuộc tính` disable-gpu` vào iGPU)
`agdpmod = pikera` | Thay thế `board-id` bằng` board-ix`. Tắt kiểm tra Board-ID trên GPU AMD Navi (dòng RX 5000 & 6000). Nếu không có điều này, bạn sẽ nhận được một màn hình đen. Không sử dụng trên Thẻ Polaris hoặc Vega.
`agdpmod = vit9696` | Tắt kiểm tra` board-id` (hoặc thêm thuộc tính `agdpmod` vào GPU bên ngoài).
`applbkl = 0` | Đối số khởi động (và thuộc tính `applbkl`) để tắt các bản vá lỗi` AppleBacklight.kext` cho IGPU. Trong trường hợp cấu hình AppleBacklight tùy chỉnh, hãy đọc [this] (https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.OldPlugins.en.md)
`gfxrst = 1` | Thích vẽ logo Apple ở giai đoạn khởi động thứ 2 thay vì sao chép bộ đệm khung. Giúp quá trình chuyển đổi giữa thanh tiến trình và màn hình nền / màn hình đăng nhập mượt mà hơn nếu gắn màn hình bên ngoài.
`ngfxgl = 1` | Tắt hỗ trợ Metal trên thẻ NVIDIA (hoặc sử dụng thuộc tính` disable-metal`)
Đối số khởi động `igfxgl = 1` | (và thuộc tính` disable-metal`) để tắt hỗ trợ Metal trên Intel.
Đối số khởi động `igfxmetal = 1` | (và thuộc tính` enable-metal`) để buộc bật hỗ trợ Metal trên Intel để kết xuất ngoại tuyến.
`-igfxvesa` | Tắt tính năng tăng tốc Đồ họa Intel để hỗ trợ kết xuất phần mềm (hay còn gọi là chế độ VESA). Hữu ích khi cài đặt macOS không bao giờ thiếu trình điều khiển đồ họa cho phần cứng cũ.
`-igfxnohdmi` | đối số khởi động (và thuộc tính `disable-hdmi-patch`) để tắt các bản vá chuyển đổi DP sang HDMI cho âm thanh kỹ thuật số.
`-cdfon` | Boot-arg (và thuộc tính `enable-hdmi20`) để kích hoạt các bản vá lỗi HDMI 2.0.
`-igfxhdmidivs` | đối số khởi động (và thuộc tính `enable-hdmi-dividers-fix`) để khắc phục vòng lặp vô hạn về việc thiết lập kết nối Intel HDMI với tốc độ xung nhịp pixel cao hơn trên các nền tảng SKL, KBL và CFL.
Đối số khởi động `-igfxlspcon` | (và thuộc tính` enable-lspcon-support`) để kích hoạt hỗ trợ trình điều khiển cho chip LSPCON tích hợp. [Đọc hướng dẫn sử dụng](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.IntelHD.en.md#lspcon-driver-support-to-enable-displayport-to-hdmi-20-output-on-igpu)
`igfxonln = 1` | đối số khởi động (và thuộc tính thiết bị `force-online`) để buộc trạng thái trực tuyến trên tất cả các màn hình.
`-igfxdvmt` | đối số khởi động (và thuộc tính `enable-dvmt-calc-fix`) để khắc phục sự cố hạt nhân do lượng bộ nhớ phân bổ trước DVMT được tính toán không chính xác trên nền tảng ICL của Intel.
`-igfxblr` | đối số khởi động (và thuộc tính `enable-backlight-register-fix`) để sửa lỗi đăng ký đèn nền trên nền tảng KBL, CFL và ICL.
`-igfxbls` | đối số khởi động (và thuộc tính `enable-backlight-Smooth`) để làm cho quá trình chuyển đổi độ sáng mượt mà hơn trên nền tảng IVB +. [Đọc hướng dẫn sử dụng](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.IntelHD.en.md#customize-the-behavior-of-the-backlight-smoother-to-improve-your-experience)
`applbkl = 3` | đối số khởi động (và thuộc tính `applbkl`) để cho phép điều khiển đèn nền PWM của card đồ họa dòng AMD Radeon RX 5000 [đọc tại đây.](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.Radeon.en.md)

#### Provided by AppleALC
Boot-args cho kext trình điều khiển âm thanh yêu thích của bạn. Tất cả các đối số khởi động Lilu cũng ảnh hưởng đến AppleALC.

| Boot-arg | Mô tả |
|: ------: | ----------- |
`alcid = layout` | Để chọn một id bố cục, ví dụ: alcid = 1
`-alcoff` | Tắt AppleALC (Chế độ khởi động `-x` và` -s` cũng sẽ tắt nó)
`-alcbeta` | Bật AppleALC trên các hệ thống không được hỗ trợ (thường là những hệ thống chưa được phát hành hoặc cũ)
`alcverbs = 1` | Cho phép hỗ trợ alc-động từ (cũng thuộc tính thiết bị alc-động từ)

Nếu bạn nhấp chuột phải vào bất kỳ đâu trong danh sách này, bạn sẽ tìm thấy nhiều boot-args khác không được đề cập ở đây::

<details><summary><strong>Screenshot</strong></summary>

![Bildschirmfoto](https://user-images.githubusercontent.com/76865553/135818786-923330d4-564a-41c6-acbf-ae16b4ac0d55.png)
</details>

### Default Boot Volume
Khối lượng khởi động mặc định được sử dụng để chỉ định mục nhập nào là mục nhập khởi động mặc định trong GUI Clover. Nó có thể được đặt thành:

- `LastBootedVolume`: Khối lượng khởi động cuối cùng sẽ được đặt làm mặc định trong GUI của Clover.
- Volume Name: Tên của tập. Ví dụ. `Macintosh`.
- GUID - ID duy nhất trên toàn cầu của ổ đĩa được hiển thị trong nhật ký khởi động, khởi động trước hoặc gỡ lỗi của Clover. Ví dụ. `57272A5A-7EFE-4404-9CDA-C33761D0DB3C`.
- Một phần của Đường dẫn thiết bị - Cũng được hiển thị trong nhật ký của Clover. Ví dụ. HD (1, GPT, 57272A5A-7EFE-4404-9CDA-C33761D0DB3C, 0x800,0xFF000).

Đĩa khởi động OS X có thể được sử dụng để khởi động lại vào một ổ đĩa khác, nhưng cho lần khởi động lại sau, DefaultVolume sẽ được sử dụng lại.

### Legacy
Khởi động di sản. Cần thiết để chạy các phiên bản Windows và Linux cũ hơn. Phụ thuộc vào phần cứng và cấu trúc của BIOS, vì vậy một số thuật toán đã được thực hiện.

Các tùy chọn là:

- `LegacyBiosDefault`: dành cho những BIOS UEFI có chứa giao thức LegacyBios.
- `PBRtest`,` PBR`, `PBRsata` - các biến thể của thuật toán khởi động PBR.

Nói chung, không thể đạt được hoạt động khởi động kế thừa vô điều kiện. Việc quên đi các hệ thống kế thừa và sử dụng các phiên bản hệ điều hành UEFI sẽ dễ dàng hơn và tốt hơn. Phiên bản cũ nhất trong số đó là Windows 7-64 và cá nhân tôi thấy không có lý do gì để gắn bó với WindowsXP. Có ai vẫn sử dụng bộ xử lý chỉ 32 bit không? Chà, chúc bạn may mắn!

### Default Loader
Ngoài `DefaultVolume`, đường dẫn của bộ tải có thể được chỉ định là DefaultLoader. Điều này cung cấp lựa chọn mục nhập mặc định chính xác hơn cho các Tập có nhiều Bộ nạp. Giá trị có thể là đường dẫn hoàn chỉnh hoặc một phần duy nhất như tên tệp.

### XMPDetection
Phát hiện Hồ sơ bộ nhớ eXtreme tốt nhất khi phát hiện bộ nhớ hoặc tắt tính năng phát hiện XMP.

### NoEarlyProgress
Loại bỏ chú giải công cụ trước khi tải giao diện trình tải, ví dụ: "Chào mừng đến với Cỏ ba lá".

### Custom Logo
Cho phép vẽ biểu trưng khởi động tùy chỉnh.


- `CÓ`– Sử dụng kiểu khởi động mặc định, Apple.
- `KHÔNG` - Tắt logo khởi động tùy chỉnh.
- `Apple` - Sử dụng màu xám mặc định trên logo Apple màu xám.
- `Thay thế` - Sử dụng thay thế màu trắng trên logo Apple màu đen.
- `Chủ đề` - Sử dụng màn hình khởi động chủ đề cho loại mục nhập - KHÔNG ĐƯỢC THỰC HIỆN.
- `Không có gì` - Không sử dụng màu nền chỉ có logo, màu xám nếu không được chỉ định bởi mục nhập tùy chỉnh.

**NOTE**: kể từ r5140.1 "Apple" và "Alternate" không hiển thị thanh tiến trình. Những cảm giác như thế này chỉ là hình nền che màn hình khởi động.

### DisableCloverHotkeys
Tắt tất cả các phím nóng trong menu bộ nạp khởi động. Bạn có thể tìm thấy danh sách tất cả các phím nóng bằng cách nhấn F1 trong menu bộ nạp khởi động.

### HibernationFixup
Khắc phục chế độ Ngủ đông trên Hệ thống không có NVRAM gốc

### NeverDoRecovery
Với công nghệ FileVault2, giờ đây có thể sử dụng phím nóng, nhưng để làm như vậy, bạn phải ghi đè các nhiệm vụ đã được thực hiện trong Clover.

### NeverHibernate
Tắt tính năng phát hiện trạng thái ngủ đông.

### RtcHibernateAware
Chìa khóa để làm việc an toàn với RTC trong chế độ ngủ đông. Chỉ yêu cầu trong macOS 10.13

### SkipHibernateTimeout
Tắt Thời gian chờ Ngủ đông.

### precisionHibernate
Điều này chỉ hoạt động nếu bạn có NVRAM đang hoạt động. Nó tương thích với công nghệ FileVault2, nơi mà cách cũ không hoạt động.

### Timeout
Đặt thời gian chờ tính bằng giây (0-30 giây), trước khi quá trình khởi động tiếp tục tải âm lượng mặc định. Bộ hẹn giờ bị vô hiệu hóa nếu người dùng nhập xuất hiện trước khi bộ hẹn giờ kết thúc.

- `-1`: đặt thời gian chờ thành vô hạn. Không có gì xảy ra cho đến khi một đầu vào của người dùng xảy ra.
- `Nhanh`: Bỏ qua GUI và khởi động từ ổ đĩa mặc định ngay lập tức, do đó không thể tương tác với người dùng - vì vậy không có cơ hội sửa chữa điều gì đó trong trường hợp xảy ra lỗi.

### SignatureFixup
 Khi ở chế độ ngủ đông, hệ thống sẽ để lại chữ ký trong hình ảnh, sau đó sẽ được kiểm tra bằng `boot.efi`. Chúng tôi muốn sửa nó bằng chìa khóa này. Có lẽ là không có gì. Bạn nên để nó vô hiệu hóa.
 
### Debug
Nếu được đặt thành `true`, nhật ký sẽ được tạo vào lần khởi động tiếp theo. Điều này sẽ làm chậm nghiêm trọng thời gian khởi động nhưng cho phép bạn tìm ra vấn đề là gì vì mỗi bước sẽ đi kèm với việc ghi debug.log vào đĩa / ổ đĩa flash. Thời gian khởi động khoảng 10 phút chỉ để vào GUI. Nhưng nếu mọi thứ bị treo, bạn có thể nhấn Đặt lại, sau đó tìm tệp `/ EFI / CLOVER / misc / debug.log`, tệp này ghi lại tất cả nhật ký cho tất cả các lần tải, miễn là tham số này được đặt.