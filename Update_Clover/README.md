# Upgrading Clover for macOS 11+ compatibility

## Why Upgrade?
`AptioMemoryFixes`trước đây của Clover không có khả năng khởi động / cài đặt macOS 11 và mới hơn. Do đó, các Bản sửa lỗi bộ nhớ của OpenCore (`OpenRuntime.efi`) đã được tích hợp để giữ cho Clover phù hợp. Vì Clover r5126, các bản sửa lỗi Bộ nhớ Aptio đã lỗi thời và không còn được hỗ trợ, do đó, nâng cấp lên phiên bản Clover mới nhất là bắt buộc để có thể cài đặt và khởi động macOS 11 trở lên.

Người dùng muốn tiếp tục sử dụng macOS Catalina trở lên nên cập nhật lên [r5123.1](https://github.com/CloverHackyColor/CloverBootloader/releases/tag/5123.1) là phiên bản cuối cùng với các bản sửa lỗi bộ nhớ aptio cũ nhưng chứa bản sửa lỗi được chờ đợi từ lâu cho `TgtBridge` đã bị hỏng trong một thời gian dài.

## Hướng dẫn này dành cho ai?
Hướng dẫn này dành cho tất cả mọi người đang cố gắng nâng cấp lên bản sửa đổi mới nhất của Clover, để họ có thể cài đặt và chạy macOS Big Sur và mới hơn trên máy của mình. Khi cập nhật Clover, có một số trở ngại trong quá trình thực hiện, chẳng hạn như xóa các bản sửa lỗi bộ nhớ cũ, trình điều khiển và chọn cài đặt chính xác cho phần "Quirks" mới được thêm vào của `config.plist`. Người dùng không muốn chạy macOS Big Sur hoặc hệ thống mới hơn trên đó không cần cập nhật Clover - mặc dù bạn có thể, theo[documentation](https://www.insanelymac.com/forum/topic/304530-clover-change-explanations/?do=findComment&comment=2751618): "New Clover sẽ hiểu config.plist cũ. Bạn không thể thay đổi nó."

## Mô tả vấn đề
Nếu bạn chỉ cập nhật Clover EFI "cũ" hiện có của mình bằng cách cài đặt `Clover.pkg` mới nhất như bạn đã sử dụng, điều này rất có thể dẫn đến bộ nạp khởi động không hoạt động được do thiếu thông số khởi động trong` config.plist` cũng như phần còn lại các tệp từ phiên bản Clover "cũ" cần được xóa trước.

## Điều kiện tiên quyết: loại bỏ các trình điều khiển lỗi thời và tránh xung đột kext
Để tránh trường hợp hệ thống không khởi động được, bạn phải dọn dẹp thư mục EFI cũ trước khi nâng cấp lên macOS 11+.

### Xóa các trình điều khiển lỗi thời / không cần thiết
Các trình điều khiển sau không còn cần thiết và phải bị xóa khi cập nhật Clover hoặc bị bỏ qua khi tạo thư mục EFI mới:

| Trình điều khiển | Mô tả | Hành động
| --------- | ----------- | ----- |
| ** `AptioMemoryFix.efi` **, </br> **` OsxAptioFix3Drv.efi` **, </br> ** `OsxAptioFixDrv.efi` **, </br> **` OcQuirks.efi` ** và ** `OcQuirks.plist` ** | Các bản sửa lỗi bộ nhớ Aptio và OCQuirks đã lỗi thời. OcQuirks là di tích từ những nỗ lực trước đó để đưa OpenCore Booter Quirks vào Clover (≤ r5122). | Xóa
** `DataHubDxe.efi` ** | Giao thức DataHub cung cấp các tham số như Mô hình OEM, FSBFrequency, ARTFrequency, nhật ký khởi động của Clover và nhiều thứ khác cho macOS mà nó không thể lấy được. Nó đã được tích hợp hoàn toàn vào Clover kể từ r5129. Các phiên bản mới hơn của Gói Clover vẫn không chứa trình điều khiển này. | Xóa
** `EmuVariableUefi.efi` ** | Cần thiết để mô phỏng NVRAM, nếu nó không có sẵn (hệ thống cũ) hoặc hoạt động không chính xác. | Xóa
** `FSInject.efi` ** | Đối với tiêm Kext. Chỉ cần thiết cho các phiên bản cũ của macOS ≤ 10.7 (Lion) có khả năng tải các kexts riêng lẻ thay vì Prelinkedkernel. Kể từ r5125, OpenCore xử lý việc chèn Kext, vì vậy FSInject đã trở nên lỗi thời | Xóa
** `SMCHelper.efi` ** | Chỉ bắt buộc khi sử dụng `FakeSMC.kext`. Nếu bạn sử dụng nó kết hợp với `VirtualSMC.efi`, nó có thể gây ra Kernel Panics ngay lập tức. Nói cách khác: FakeSMC.kext + SMCHelper.efi = <span style ="color: green"> tốt </span>, VirtualSMC.kext + VirtualSMC.efi = <span style ="color: green">tốt</ span >; bất kỳ kết hợp nào khác của cả hai: <span style="color:red">bad</span>. Ngày nay, chỉ sử dụng VirtualSMC.kext là đủ và được khuyến khích. | Xóa bỏ

### Checking and Updating Kexts
Kexts lỗi thời, không tương thích và / hoặc trùng lặp (và các biến thể của chúng) có thể gây ra lỗi khởi động, loạn nhân và mất ổn định hệ thống chung. Do đó, bạn phải luôn cập nhật kexts của mình để có khả năng tương thích tối đa với macOS và Clover! Bạn có thể sử dụng Kext-Updater để tải xuống kext mới nhất và các tệp liên quan đến Bootloader khác.

Nếu bạn đang sử dụng nhiều Kexts (thường là trên Máy tính xách tay), hãy xem bên trong chúng (nhấp chuột phải và chọn "Hiển thị nội dung gói") để kiểm tra xem chúng có bao gồm thêm kexts (dưới dạng "Plugin") hay không và đảm bảo rằng không các bản sao tồn tại trong thư mục "kexts" - Kexts cho HID, WiFi và Bluetooth đã được lưu ý.

Nếu bạn hoảng sợ mà không thể cô lập, hãy tạm thời di chuyển tất cả các ke không thiết yếu sang thư mục "Tắt" để khắc phục sự cố bằng cách bắt đầu với một bộ Kexts tối thiểu để hệ thống chạy. Khi nó chạy, hãy đặt từng Kexts đã bị vô hiệu hóa trở lại lần lượt, khởi động lại và lặp lại cho đến khi bạn tìm ra thủ phạm gây ra sự hoảng loạn và tìm giải pháp (chế độ tiết là bạn của bạn).

Dưới đây là một số ví dụ về Kexts mà tôi đã gặp sự cố khi cập nhật:

- **VoodooPS2Controller.kext**: có thể gây ra Kernel Panic nếu một trong các Plugin của nó (VoodooInput.kext, VoodooPS2Mouse.kext, VoodooPS2Trackpad.kext và VoodooPS2Keyboard.kext) cũng có mặt ở cấp gốc của Thư mục "kexts".
- **AirportBrcmFixup.kext**: Kext này chứa 2 Plugin là `AirPortBrcm4360_Injector` và` AirPortBrcmNIC_Injector.kext`. Khi sử dụng AirPortBrcmFixup, bạn phải chỉ sử dụng một trong những plugin này, không phải cả hai! Sử dụng cả hai có thể khiến quá trình khởi động bị đình trệ vô thời hạn. Trên hết, `AirPortBrcm4360_Injector` không được macOS Big Sur hỗ trợ và vẫn phải tắt. Trong OpenCore, bạn chỉ có thể vô hiệu hóa Kext trong cấu hình. Vì cấu hình Clover không hỗ trợ kiểm soát trình tự tải kext, bạn phải xóa nó khỏi chính Kext (nhấp chuột phải vào AirportBrcmFixup, chọn "Hiển thị nội dung gói"> "Plugin").
- **BrcmPatchRAM** và sự kết hợp không tốt giữa các kexts đi kèm với nó cũng có thể gây ra sự cố. Không sử dụng BlueToolFixup.kext và BrcmBl BluetoothInjector.kext cùng nhau. Cái trước cần thiết để bật Bluetooth trong macOS Monterey trong đó cái sau được sử dụng trong các phiên bản macOS trước đó.

## Building a new EFI folder

1. Chuẩn bị một ổ đĩa flash USB. Định dạng nó thành FAT32 (MBR). Trước tiên, chúng tôi sẽ sử dụng nó để kiểm tra Thư mục EFI đã cập nhật của chúng tôi, trước khi sao chép nó vào ESP trên HDD.
2. Tải xuống [Bản phát hành cỏ ba lá mới nhất](https://github.com/CloverHackyColor/CloverBootloader/releases) dưới dạng kho lưu trữ .zip để cập nhật thủ công.
3. Tải xuống cả `CloverConfigPlistValidator.zip`.
4. Giải nén cả hai kho lưu trữ zip.
5. Xem qua `CloverV2/EFI/CLOVER/drivers/off/UEFI` và các thư mục con của nó. Bên trong, bạn sẽ tìm thấy các Trình điều khiển này: </br>
! [Bildschirmfoto 2021-10-05 um 10 32 30](https://user-images.githubusercontent.com/76865553/136025337-d12b41ac-b3f6-4c3f-9a31-e61294daf01a.png)</br>
6. Di chuyển các trình điều khiển sau từ "thợ lặn / Tắt" sang "/ trình điều khiển / UEFI": </br>
    - ApfsDriverLoader.efi,
    - VBoxHfs.efi (hoặc HfsPlus.efi, nhanh hơn) và
    - OpenRuntime.efi </br>
Bây giờ chúng tôi có một bộ Trình điều khiển tối thiểu: </br>
! [Trình điều khiển](https://user-images.githubusercontent.com/76865553/136026914-af63dce9-a505-4b61-8ad4-0d14348fac37.png)</br>
Các tệp được gắn thẻ màu xám ở đó theo mặc định và rất có thể không cần thiết đối với các hệ thống dựa trên UEFI. Tôi sẽ chuyển lần lượt chúng vào thư mục "off" để vô hiệu hóa chúng và kiểm tra xem hệ thống có còn khởi động từ ổ flash USB mà không có chúng hay không. `AudioDXE.efi` chỉ cần thiết để phát lại các tệp âm thanh như chuông khởi động - vì vậy nếu bạn không sử dụng bất kỳ tệp nào, bạn có thể xóa / tắt nó. Như đã đề cập trước đó, PHẢI xóa `SMCHelper.efi` khi sử dụng` VirtualSMC.kext`!
7. Tiếp theo, sao chép các tệp / thư mục sau từ thư mục EFI hiện có của bạn: </br>
- ** Kexts ** (tất nhiên được cập nhật lên phiên bản mới nhất hiện có),
- **. aml ** Các tệp từ thư mục "ACPI / đã vá"
- ** config.plist **
8. Chạy Clover Configurator và cập nhật nó lên Phiên bản mới nhất. Bây giờ nó sẽ bao gồm một phần mới ở dưới cùng có tên là `Quirks`.
9. Mở `config.plist` của bạn
10. Nhấp vào "Quirks". Nó sẽ giống như thế này (trong phiên bản mới nhất của Clover Configurator, phần Quirks được chia thành các phần "Booter" và "Kernel" để có cái nhìn tổng quan hơn):
![](https://user-images.githubusercontent.com/76865553/135844035-1689a11a-6512-4008-80ea-e89f07a55367.png)
11. Head over to the [**OpenCore Install Guide**](https://dortania.github.io/OpenCore-Install-Guide/prerequisites.html) and pick the guide for your CPU Family and Platform.
12. Chuyển đến Phần "Khởi động". Nó chứa tất cả các Booter Quirks bắt buộc được tô màu xanh lá cây trong ảnh chụp màn hình. Đảm bảo mở hộp "thông tin chuyên sâu hơn" để xem họ làm gì. Tìm các tùy chọn cho hệ thống của bạn và đánh dấu chúng trong Clover Configurator.
13. Tiếp theo, chuyển đến phần "Kernel" trong hướng dẫn của Dortania và sao chép các cài đặt từ "Quirks" và "Scheme". Một lần nữa, hãy nhớ mở phần "chuyên sâu hơn" để tìm tất cả các cài đặt cần thiết. </br>
    **LƯU Ý**: một số "Kernel Quirks" của OpenCore có tên khác và nằm trong phần "Kernel and Kext Patches" của Clover Configurator. Trong hầu hết các trường hợp, bạn đã bật cài đặt chính xác, nếu không hệ thống của bạn sẽ không khởi động trước đó. Nhưng tốt nhất là bạn nên kiểm tra lại xem bạn đã bật các cài đặt có thể không cần thiết hay chưa. Bao gồm các: ![](https://user-images.githubusercontent.com/76865553/135859628-34f6be51-7a20-4461-900e-0c72fbdcba51.png)</br>
14. Cuối cùng, bạn có thể xóa tất cả các Bản vá giới hạn cổng USB không dùng nữa khỏi cấu hình của mình vì điều này có thể được xử lý bởi `XhciPortLimit` Quirk (đối với macOS ≥ Catalina, bạn sẽ cần một USBPort.kext).
15. Khi bạn đã đánh dấu tất cả các câu hỏi cần thiết, hãy lưu cấu hình của bạn.

### Xác thực config.plist và sửa lỗi

Bắt đầu từ phiên bản r5134, Clover hiện bao gồm báo cáo lỗi tương tự như OpenCore, hiển thị lỗi cấu hình trước khi menu khởi động thực sự xuất hiện. Làm như sau để xác thực cấu hình của bạn và sửa lỗi cấu hình:

1. Open Terminal
2. Kéo `CloverConfigPlistValidator` vào đó và nhấn phím mũi tên phải một lần để đường dẫn tệp không còn được tô sáng nữa
3. Tiếp theo, kéo và thả config.plist cỏ ba lá của bạn vào cửa sổ đầu cuối. Đảm bảo có khoảng trống giữa 2 đường dẫn tệp
4. Nhấn "Enter"
5. Kiểm tra kết quả. Nếu nó có nội dung: "Bảng xếp hạng của bạn trông thật tuyệt vời. Làm tốt lắm!", Thì bạn không cần phải làm gì khác.
6. Nếu có lỗi hiển thị trong nhật ký, hãy mở cả `config.plist` của bạn và` config-sample.plist` có trong gói Clover trong trình chỉnh sửa plist và so sánh cấu trúc của (các) phần được tham chiếu bởi trình xác thực. Tìm kiếm bất kỳ sự khác biệt nào (như định dạng, các tính năng đã xóa, v.v.) và sửa chúng.
7. Lưu cấu hình của bạn.
8. Kiểm tra lại lỗi
9. Lặp lại so sánh, sửa chữa, lưu và kiểm tra lại cho đến khi tất cả các vấn đề được giải quyết

### Đang kiểm tra cấu hình (mới) của bạn

- Sao chép Thư mục EFI mới vào thư mục gốc của Ổ đĩa flash USB của bạn và thử khởi động từ đó.
- Trong menu Khởi động của Clover, thực hiện Đặt lại NVRAM (F11) để loại bỏ tàn dư có thể có của các Bản sửa lỗi bộ nhớ Aptio trước đó
- Khởi động lại một lần nữa và sau đó khởi động macOS.

Nếu nó khởi động được, bạn có thể gắn phân vùng ESP của ổ cứng, sao lưu Thư mục EFI cũ của bạn, xóa nó và đưa vào từ ổ USB Flash của bạn để thực hiện các thay đổi vĩnh viễn.

## Further Resources and Troubleshooting:

- Clover Config Guide (up to Coffeelake, excluding Quirks): [**Hackintosh Vanilla Desktop Guide**](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/)
- Để tìm hiểu thêm về cách OpenCore khác với Clover và xem cài đặt, trình điều khiển và tính năng nào tương thích, hãy xem [**Clover Conversion Guide**](https://github.com/dortania/OpenCore-Install-Guide/tree/master/clover-conversion).
- [**Clover Changelog**](https://www.insanelymac.com/forum/topic/304530-clover-change-explanations/)
- If you get Kernel Panics: head over to the [**OpenCore Troubleshooting Guide**](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/troubleshooting.html) và xem phần "Sự cố khởi động OpenCore" và "Sự cố không gian hạt nhân" để tìm thông báo lỗi của bạn và các bản sửa lỗi có thể giải quyết.
- Khắc phục sự cố mạng sau khi cập nhật Clover: Hãy xem [**this**](https://www.insanelymac.com/forum/topic/345789-guide-how-to-update-clover-for-bigsur-compatibility-and-beyond-using-openruntime-and-quirks-r5123/page/2/?tab=comments#comment-2751614)
- Check the [**ACPI section**](https://github.com/5T33Z0/Clover-Crate/tree/main/ACPI) để tìm hiểu thêm về tất cả các Phần ACPI quan trọng của Cỏ ba lá.

Chúc may mắn!

**LƯU Ý**

- Đối với Big Sur và mới hơn, hãy xóa PreBoot Volume khỏi Phần "Ẩn" của GUI vì macOS yêu cầu nó để khởi động!
- Ngay cả khi bạn không muốn nâng cấp macOS qua Catalina, bạn nên nâng cấp lên Clover mới vì tính năng chèn kext qua Open Runtime rõ ràng hơn và bạn có nhiều quyền kiểm soát hơn đối với các bản sửa lỗi bộ nhớ được áp dụng. Hệ thống cũng khởi động nhanh hơn.