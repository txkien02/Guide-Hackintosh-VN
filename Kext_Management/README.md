# Kext Management in Clover
## About Kexts
Kexts là từ viết tắt của "Kernel Extensions". Chúng được yêu cầu bởi Hệ điều hành mac. Hầu hết chúng là trình điều khiển thiết bị của một số loại hoặc trình điều khiển giả cho các thiết bị bị thiếu trong phần cứng PC, chẳng hạn như `FakeSMC` hoặc` VirtualSMC` mô phỏng tất cả các Bộ điều khiển quản lý hệ thống quan trọng không tồn tại trên phần cứng PC nhưng được yêu cầu bởi macOS để chạy. Chúng tôi cần bổ sung các kexts này vì hệ thống của Apple không hỗ trợ thiết bị của chúng tôi hoặc chúng tôi muốn thêm chức năng mới. Đối với Linux và Windows, không cần Kexts.
## About the `kexts` folder used by Clover
Kể từ bản sửa đổi 3281, Clover tải kexts có điều kiện, dựa trên các thư mục mà kexts được đặt trong đó. Hệ thống phân cấp như sau:

1. Theo mặc định, tất cả các kexts được đặt trong thư mục `kexts \ Other` sẽ được tải trước tiên và cho * bất kỳ phiên bản * nào của macOS.
2. Tiếp theo, các Kexts nằm trong các thư mục con tương ứng với các phiên bản macOS cụ thể sẽ được tải (ví dụ: `10.15`, `11`, `12`,v.v.). Đây là một tính năng khá tiện dụng vì một số phiên bản macOS yêu cầu các biến thể và kết hợp kexts khác nhau để làm cho một tính năng / thành phần hoạt động (ví dụ như kext mạng như `BrcmPatchRAM`). Bằng cách này, bạn không phải lo lắng về việc sắp xếp các Kexts xung quanh giữa các thư mục `Off` và` Other` khi sử dụng các phiên bản macOS khác nhau.

** LƯU Ý **: Cách tiếp cận quản lý kext này tương tự như các tham số `MinKernel` và` MaxKernel` được OpenCore sử dụng, nó không đẹp bằng vì điều này sẽ tạo ra các kext trùng lặp mà tất cả đều phải được cập nhật khi có phiên bản mới của một kext có sẵn.

## Example of conditional Kext Loading
In the screenshot below, you see an example of conditional kext loading used on a Laptop:
![kexts](https://user-images.githubusercontent.com/76865553/147611657-b56b13b3-0b5b-440e-9eb8-3a48849d903f.png)
Như bạn có thể thấy, hầu hết các kexts nằm trong thư mục `kexts \ Other`, nhưng có các kext bổ trợ được tải cho các phiên bản macOS khác:

- đối với macOS `10.13`, cần có thêm kexts` BrcmPatcRAM2` và `NoTouchID`.
- đối với macOS `10.14` chỉ cần có` BrcmPatcRAM2.kext` vì sự cố yêu cầu `NoTouchID` đã được khắc phục kể từ macOS Mojave.
- đối với macOS `10.15` và` 11`, Thẻ Broadcom Wifi / BT của tôi yêu cầu `BrcmPatcRAM3.kext` và` BrcmBl BluetoothInjector.kext` để hoạt động.
- Mặt khác, đối với macOS `12`, sử dụng ngăn xếp Bluetooth hoàn toàn mới, thay vào đó chỉ cần có` BlueToolFixup.kext`.

Lợi ích của việc quản lý kexts theo cách này là bạn không phải tạo các thư mục EFI khác nhau để chạy các phiên bản macOS khác nhau trên cùng một hệ thống. Tất cả những gì bạn có thể cần là một `config.plist` khác với SMBIOS khác hoặc các bản vá bổ sung / khác được yêu cầu để phiên bản macOS đó chạy. Vì bạn có thể thay đổi cấu hình từ bên trong Clover Bootmenu, đây là một cách khá tiện dụng để quản lý các phiên bản macOS khác nhau chỉ với một Thư mục EFI.
