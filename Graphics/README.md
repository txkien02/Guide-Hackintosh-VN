# Graphics
![Graphics](https://user-images.githubusercontent.com/76865553/136713622-7300a5e5-de05-413a-b748-579b95a36d58.jpeg)

Nhóm thông số này dùng để đưa các Thuộc tính vào cho nhiều loại card đồ họa khác nhau, trên bo mạch và rời. Có nhiều tham số thực sự được đưa vào, nhưng chúng chủ yếu là hằng số, một số được tính toán, một số được định nghĩa trong bảng nội bộ và chỉ có rất ít tham số được nhập thông qua cấu hình.

** CHÚ Ý! ** Hầu hết các tùy chọn có sẵn trong phần này đã lỗi thời vào năm 2021. Thay vào đó, các bản vá bộ đệm khung được nhập thông qua `Devices/Properties` . Các cài đặt duy nhất vẫn hữu ích là các mục trong menu thả xuống của`ig-platform-id` cho Intel iGPUs (tối đa Coffeelake).

## Inject
Có thể đưa ra hai chục tham số, được tính toán không chỉ dựa trên kiểu thẻ mà còn dựa trên các đặc điểm bên trong của nó sau khi phân tích bios video của GPU.

Đối với thẻ Nvidia, `NVCAP` được tính toán, đối với đồ họa trên bo mạch của Intel, hàng chục thông số được áp dụng, đối với thẻ ATI, các thông số về đầu nối được đưa vào. Để liệt kê tất cả chúng sẽ mất hơn hai trăm trang. Hơn nữa, đối với các GPU rời hiện đại, bạn không cần những thứ này nữa, vì Apple đảm bảo rằng chúng hoạt động tốt.

Bạn có thể kích hoạt việc đưa các tham số dựa trên các nhà cung cấp:

- `Inject ATI`
- `Inject Intel`
- `Inject NVidia` 

**LƯU Ý**: Các thông số này đã được sử dụng trước khi có sự ra đời của `Whatevergreen.kext` (WEG). Bây giờ WEG thực hiện hầu hết các tùy chỉnh đồ họa cho bạn, bạn nên tắt tất cả các lần tiêm này.

## Dual Link
Giá trị mặc định là `1`, nhưng đối với một số cấu hình cũ hơn, điều này sẽ gây ra sự cố. Trong trường hợp này, hãy đặt nó thành `0`.

## EDID
Dữ liệu nhận dạng màn hình mở rộng (EDID) là định dạng siêu dữ liệu dành cho thiết bị hiển thị để mô tả khả năng của chúng với nguồn video (ví dụ: cạc đồ họa). Định dạng dữ liệu được xác định bởi một tiêu chuẩn được xuất bản bởi Hiệp hội Tiêu chuẩn Điện tử Video (VESA). Đôi khi giá trị này là cần thiết để khắc phục các sự cố như trục trặc đồ họa, v.v.

Các thông số sau có thể được điều chỉnh:

- `VendorID`
- `ProductID`
- `HorizontalSyncPulseWidth`: Khắc phục sự cố tám quả táo. Xem 
` EightApple` Fix trong & rarr; Kernel Kext và Patches.
- `VideoInputSignal`

## FB Name
Thông số này dành riêng cho Thẻ ATI Radeon, trong đó tồn tại khoảng ba chục bộ đệm khung khác nhau không tuân theo bất kỳ mẫu cụ thể nào. Clover tự động chọn tên thích hợp nhất từ ​​bảng trên hầu hết các thẻ đã biết.

Tuy nhiên, những người dùng khác của cùng một thẻ bị thách thức, họ muốn có một tên khác. Vì vậy, hãy chọn một từ menu thả xuống.

## Load VBios
Tải bios video từ một tệp. Nó phải có trong `EFI/CLOVER/OEM/xxx/ROM` hoặc `EFI/CLOVER/ROM` và được đặt tên là `vendor_device.rom`, ví dụ: `1002_68d8.rom`. Điều này đôi khi có ý nghĩa nếu bạn sử dụng Vbios đã được vá. Kể từ r3222, các tên tệp dài hơn bao gồm nhà cung cấp phụ và bản sửa đổi phụ cũng được hỗ trợ. Ví dụ `10de_0f00_1458_3544.rom`.

Điều này cũng có thể được sử dụng để đưa thông tin thiết bị của thẻ Radeon di động vào hệ thống, nếu macOS không thể phát hiện ra nó. Clover sẽ lấy VBios từ bộ nhớ cũ tại địa chỉ `0xc0000` để đưa nó vào hệ thống và radeon di động sẽ bật!

Đối với máy tính có BIOS chỉ dành cho UEFI, không có Vbios trên địa chỉ kế thừa. Hãy đưa nó vào hồ sơ và chờ các giải pháp mới…

## NVCAP
Thông số này dành cho card màn hình Nvidia và cấu hình các loại cũng như cách sử dụng các cổng video.
Giá trị bao gồm 40 chữ số thập lục phân in hoa phải được tính toán dựa trên phân tích VBIOS của thẻ video của bạn. Thông tin thêm [** tại đây **](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/nvidia-patching/#nvcap).

## NvidiaGeneric
Nếu được bật, thì thay vì tên "Gigabyte Geforce 7300LE", "NVIDIA Geforce 7300LE" sẽ được sử dụng.

## NvidiaSingle
Đối với hệ thống có nhiều hơn một card đồ họa Nvidia. Nếu được bật, chỉ một GPU Nvidia được đưa vào.

## NvidiaNoEFI
Thêm thuộc tính NVDA vào bộ phun Nvidia. Giải thích [tại đây](https://www.insanelymac.com/forum/topic/306156-clover-problems-and-solutions/page/84/?tab=comments#comment-2443062).

## PatchVbios
Sửa VBios tại địa chỉ `0xC0000` để nó hỗ trợ độ phân giải tối đa cho màn hình được kết nối. Ví dụ: `EDID` của màn hình có chế độ 1920x1080, nhưng VBios thì không. Clover sẽ quy định nó là chế độ đầu tiên và bắt đầu sử dụng nó. Nếu bản thân màn hình không cung cấp `EDID`, nó có thể được đưa vào như mô tả ở trên.

Đã có trường hợp bật bản vá lỗi này lên gây ra tình trạng loạn, màn hình đen khi cố khởi động. Đối với lần thử đầu tiên, hãy tắt cài đặt này. Sử dụng giá trị từ tệp config.plist cho bản vá. Nếu quá trình tự động hóa mắc lỗi, bạn có thể viết bản vá VideoBios theo cách thủ công, sử dụng thuật toán Tìm / Thay thế tiêu chuẩn.
## RadeonDeInit
Phím này hoạt động với thẻ ATI / AMD Radeon 6xxx trở lên. Hoặc có thể là 5xxx, chưa thấy bất kỳ đánh giá nào. Nó sửa nội dung của thanh ghi GPU để thẻ được khởi tạo đúng cách và trình điều khiển MacOSX hoạt động với nó như dự định.

## ig-platform-id
Tham số này là bắt buộc để định cấu hình thẻ video trên bo mạch "Intel HD Graphics xxxx". Chọn id chính xác cho CPU của bạn từ menu thả xuống trong Clover Configurator. </br>**LƯU Ý**: Kể từ năm 2021, đồ họa trên bo mạch được định cấu hình trong `Devices/Properties`, sử dụng ig-platform-id chính xác trong số các thông số bổ sung để định cấu hình trình kết nối, VRam đã sử dụng, v.v.

## VRAM
Dung lượng bộ nhớ video tính bằng MB. Trên thực tế, nó được phát hiện tự động, nhưng bạn có thể điều chỉnh bằng tay. Trong thực tế, tôi không thể nhớ một trường hợp nào mà tham số này đã giúp ích cho bất kỳ ai theo bất kỳ cách nào. Nếu bạn thấy 7Mb, đừng cố thay đổi thông số này, vô ích. Bạn cần có một thẻ video. Ví dụ đối với Radeon di động, có một mẹo sử dụng `LoadVBios=true` và bộ nhớ sẽ chính xác.