# Quirks

`Quirks` là một tập hợp các tính năng để cấu hình Cài đặt Booter và Kernel trong Clover. Công nghệ cơ bản là `OpenRuntime.efi` của OpenCore đã được tích hợp hoàn toàn vào Clover kể từ r5126. Nó thay thế các `AptioMemoryFixes` đã sử dụng trước đây không còn khả năng khởi động các phiên bản macOS mới hơn. Vì vậy, nâng cấp lên r5126 trở lên là bắt buộc để có thể cài đặt và khởi động macOS Big Sur và mới hơn với Clover.

<chi tiết>
<summary> <strong> Bối cảnh: Từ bản sửa lỗi bộ nhớ Aptio đến bản sửa lỗi </strong> </summary>

Sự phát triển của trình điều khiển để điều chỉnh bộ nhớ UEFI BIOS Aptio (của AMI) bởi Dmazar đã đánh dấu sự khởi đầu của kỷ nguyên UEFI Boot cho Clover.
Chức năng Allocate trong BIOS này phân bổ bộ nhớ trong các thanh ghi thấp hơn, nhưng để khởi động macOS, bộ nhớ dưới phải trống. Vấn đề này không chỉ ảnh hưởng đến bộ nhớ mà còn ảnh hưởng đến `boot.efi`, ảo hóa địa chỉ, con trỏ, hàm, v.v. Chính Dmazar đã tìm ra cách giải quyết chúng. Điều này đã trở thành trình điều khiển `OsxAptioFixDrv.efi`.

Trong một thời gian dài sau khi Dmazar rời đi, không ai động đến chiếc xế hộp này cho đến khi vit9696 tiến hành đại tu nó. Trước hết, anh ấy đã thực hiện các thay đổi đối với trình điều khiển để nó có thể sử dụng NVRAM gốc trên nhiều chipset (BIOS), điều mà trước đây không thể thực hiện được. Tiếp theo, anh ta chia trình điều khiển mới (`OpenRuntime.efi`) thành ** biểu thức ngữ nghĩa (quirks), người dùng có thể bật và tắt trình điều khiển này nếu trình tải OpenCore được sử dụng **.

ReddestDream, một lập trình viên đã quyết định làm cho `OpenRuntime.efi` hoạt động với Clover bằng cách nào đó, đã tạo một trình điều khiển riêng biệt (` OcQuirks.efi`), hoạt động cùng với `OpenRuntime.efi` và một` OcQuirks.plist` bổ sung để lưu trữ tất cả các cài đặt. Sự kết hợp này sau đó có thể được sử dụng bởi Cỏ ba lá.

Tiếp theo, tôi (Slice) đã tích hợp mã nguồn `OpenRuntime.efi` vào kho lưu trữ của tôi vì nó là Mã nguồn mở, vì vậy tôi có thể thực hiện chia nhỏ. Cuối cùng, tôi đã sao chép và hiện đại hóa chúng để thay vì một tệp .plist riêng biệt, có thể sử dụng cùng một Clover `config.plist` và Quirks cũng có thể được thay đổi từ bên trong GUI của bộ nạp khởi động.
</details>

Trong oder để thiết lập chính xác các Quirks này, bạn cần làm theo hướng dẫn dành cho dòng CPU của mình trong [**Hướng dẫn cài đặt OpenCore**](https://dortania.github.io/OpenCore-Install-Guide/). Cụ thể, các phần sau:

- **Booter > Quirks**
- **Kernel > Quirks**
- **Kernel > Scheme**
- **UEFI > Output: `ProvideConsoleGop`**: Nằm dưới" GUI "trong cấu hình Clover.

## Booter Quirks
Ảnh chụp màn hình sau đây cho thấy phần phụ Booter Quirks mới trong Clover Configurator 5.19.0, giúp xử lý dễ dàng hơn nhiều so với các phiên bản trước:

![booter](https://user-images.githubusercontent.com/76865553/145463363-244f28cc-12d6-437a-9dc5-b6b0d791d080.png)


Ví dụ trên cho thấy Booter Quirks cho CPU Máy tính để bàn Intel Cometlake Thế hệ thứ 10 được lấy từ[this section](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#booter) của Hướng dẫn Cài đặt OpenCore. Đừng chỉ sao chép những thứ này - hãy sử dụng đúng những thứ cần thiết cho CPU của bạn!

## Kernel Quirks, Emulate and Scheme
Nếu bạn nhấp vào "Kernel", bạn sẽ thấy những Kỳ quặc này (cũng dành cho Sao chổi thế hệ 10):
![KernelQ](https://user-images.githubusercontent.com/76865553/139507546-7c49d3da-85c6-4253-ae8b-865e39ddcb3b.png)

YBạn sẽ tìm thấy các cài đặt tương ứng cho CPU của mình trong phần "Kernel" của Hướng dẫn Dortania như đã đề cập trước đó. Thật không may, một số Kernel Quirks không nằm trong phần `Quirks` mà nằm trong phần" Kext and Kernel Patches "và - để gây nhầm lẫn hơn nữa - có tên khác với trong OpenCore:
![Qnames](https://user-images.githubusercontent.com/76865553/139507628-4dbc5d58-a823-4cd7-a739-9945dd3a2e94.png)

Người dùng của Clover < r5126 có thể theo dõi  [**Clover Upgrade Guide**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/Update_Clover) để thay thế `AptioMemoryFixes` đã lỗi thời bằng` OpenRuntime.efi` và thêm các Quirks cần thiết.
## Câu hỏi bổ sung
Phần này liệt kê các câu hỏi mới hoặc chưa được ghi lại hoặc chưa có sẵn trong Clover Configurator hoặc đáng chú ý.

### ForceOcWriteFlash
Đã thêm vào phiên bản beta r5142. Đó là một OpenCore Quirk khác được tích hợp vào Clover. Sao chép khóa từ phần `Quirks` của config-sample.plist vào config.plist của bạn vì Clover Configurator chưa hỗ trợ nó (chưa). Từ Tài liệu OpenCore:

> Cho phép ghi vào bộ nhớ flash cho tất cả các biến hệ thống NVRAM do OpenCore quản lý. Giá trị này nên bị vô hiệu hóa trên hầu hết các loại chương trình cơ sở nhưng vẫn có thể định cấu hình được để tính đến phần chương trình cơ sở có thể gặp sự cố với lỗi tràn bộ nhớ thay đổi dễ bay hơi hoặc tương tự. Sự cố khởi động trên nhiều hệ điều hành có thể được quan sát thấy trên ví dụ: Lenovo Thinkpad T430 và T530 không có điều này. Các biến Apple liên quan đến Khởi động an toàn và chế độ ngủ đông được miễn trừ điều này vì lý do bảo mật. Hơn nữa, một số biến OpenCore được miễn vì các lý do khác nhau, chẳng hạn như nhật ký khởi động do tùy chọn người dùng có sẵn và tần suất TSC do các vấn đề về thời gian. Khi bật tùy chọn này, bạn có thể phải đặt lại NVRAM để đảm bảo đầy đủ chức năng.

### Cung cấp Console Gop
`SupplyConsoleGop` là một OpenCore Quirk khác đã được triển khai vào Clover. Cho đến r5128, nó nằm trong phần `Quirks` với tên gọi là` SupplyConsoleGopEnable`. Kể từ đó, nó đã được chuyển đến phần `GUI` vì nó liên quan đến Giao diện người dùng. Để tìm hiểu xem bạn có cần nó hay không, hãy xem Hướng dẫn cài đặt OpenCore, cụ thể là phần `` UEFI> Output`. ** Gợi ý **: nó luôn được bật.

### Thay đổi kích thướcAppleGPUBars
`ResizeAppleGpuBars` Quirk là Quirk mới nhất được thêm vào Clover trong r5142. Bạn cần sao chép thủ công nó từ sample-config.plist vào cấu hình của mình vì nó chưa được áp dụng trong Clover Configurator. Nó giới hạn kích thước GPU PCI BAR cho macOS đến giá trị được chỉ định hoặc thấp hơn nếu nó không được hỗ trợ. Khi bit của Khả năng được đặt, nó chỉ ra rằng Hàm hỗ trợ hoạt động với BAR có kích thước là (2 ^ Bit) MB. `ResizeGpuBars` phải là một giá trị nguyên trong khoảng từ` -1` đến `19`.

: cảnh báo: ** CẢNH BÁO **:
> Không đặt `ResizeAppleGpuBars` thành bất kỳ thứ gì ngoại trừ` 0` nếu bạn đã bật thanh thay đổi kích thước trong BIOS. `9` và` 10` sẽ gây ra lỗi khi thức giấc và 8 sẽ gây ra việc sử dụng bộ nhớ quá mức trên một số GPU mà không có bất kỳ lợi ích hữu ích nào. Nó sẽ luôn luôn là `0`. Không quan trọng bạn có GPU nào, tất cả đều hỗ trợ tính năng này từ đầu những năm 2010, chỉ không tăng hiệu suất.
> **Source**: [Vit9696](https://www.insanelymac.com/forum/topic/349485-how-to-opencore-074-075-differences/?do=findComment&comment=2770810)

**Formula**: 2^Bit = ApppleGPUBars Size in MB

| PCI BAR Size | VALUE in OC|
|-------------:|:----------:|
| Disabled|-1|
|1 MB|0|
| 2 MB|1|
| 4 MB|2| 
| 8 MB|3|
| 16 MB|4|
| 32 MB|5|
| 64 MB|6|
| 128 MB|7|
| 256 MB|8|
| 512 MB|9|
| 1 GB°|10°|
| 2 GB|11|
| 4 GB|12|
| 8 GB|13|
| 16 GB|14|
| 32 GB|15|
| 64 GB|16|
| 128 GB|17|
| 256 GB|18|
| 512 GB|19|

`°`Maximum for macOS.

Trước khi bạn thay đổi bất kỳ giá trị nào trong số này, hãy nghiên cứu xem GPU của bạn có hỗ trợ BAR Resize hay không và kiểm tra kích thước được hỗ trợ bằng các công cụ như `HWiNFO` trên PC.
