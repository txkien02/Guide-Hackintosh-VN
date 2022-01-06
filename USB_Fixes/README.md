# Fixing USB issues

## Background
Trong macOS, số lượng cổng USB khả dụng bị giới hạn ở mức 15. Nhưng vì các bo mạch chủ hiện đại cung cấp tới 26 cổng cho mỗi bộ điều khiển USB, điều này có thể trở thành một vấn đề khi cố gắng làm cho các ổ cắm USB hoạt động bình thường. Nếu các cổng không được chỉ định chính xác, các thiết bị USB bên trong và bên ngoài sẽ mặc định ở tốc độ USB 2.0 hoặc hoàn toàn không hoạt động.

Vì một đầu nối USB 3 vật lý (màu xanh lam) thực sự hỗ trợ 2 giao thức USB, nó yêu cầu 2 cổng: một cổng gọi là "HS" cho "tốc độ cao" - thực tế là USB 2.0 và một cổng gọi là "SS" cho "siêu tốc độ", đó là USB 3.0. Nói cách khác: bạn thực sự có thể ánh xạ 7 Cổng USB 3.0 vật lý, hỗ trợ các giao thức USB 2.0 và 3.0 - và đó là về nó. Ở thời điểm này, USB 3.2 thậm chí không nằm trong phương trình như Apple lo ngại. Vì vậy, bạn phải quyết định cổng nào bạn sẽ sử dụng và ánh xạ chúng theo cách thủ công.

## Xóa giới hạn cổng USB và ánh xạ các cổng USB
Cách giải quyết là tăng giới hạn cổng USB và sử dụng các công cụ bổ sung để tạo kext không mã bao gồm bản đồ Cổng USB với 15 cổng bạn chọn. Trước macOS Catalina, bạn có thể sử dụng câu hỏi `` XhciPortLimit` 'để kích hoạt 26 Cổng và bạn đã làm tốt. Kể từ macOS Catalina, bạn cần phải ánh xạ các cổng USB để thiết bị ngoại vi của bạn hoạt động chính xác. Có hai phương pháp để thực hiện việc này:

### Phương pháp 1: Ánh xạ Cổng USB với Công cụ (chỉ Intel)

#### Ánh xạ các cổng USB trong macOS ≤ 11.2
Lên đến macOS 11.3, bạn có thể sử dụng các công cụ để tạo USBPorts.kext. Để làm như vậy, hãy làm theo Hướng dẫn chính thức của Dortania: [** Bản đồ cổng USB với USBMap **](https://dortania.github.io/OpenCore-Post-Install/usb/system-preparation.html). 
hướng dẫn của anh ấy được viết cho OpenCore nhưng các nguyên tắc tương tự có thể được áp dụng khi sử dụng Clover.

Cá nhân tôi thích [** Hackintool **](https://github.com/headkaze/Hackintool) hoặc ánh xạ. Tuy nhiên, phương pháp này chỉ hoạt động đối với hệ thống Intel. Nhưng trên các hệ thống AMD, nó không thực sự là một vấn đề, vì các bo mạch này thường chỉ có khoảng 10 cổng USB trên mỗi bộ điều khiển, nằm trong giới hạn 15 cổng cho mỗi bộ điều khiển của macOS.

#### Ánh xạ các cổng USB trong macOS 11.3+ hoặc Windows
Vì Quirk `XhciPortLimit` cần thiết để ánh xạ các cổng USB không còn hoạt động trong phiên bản macOS 11.2 nữa, bạn cần thực hiện một cách tiếp cận khác. Phương pháp đơn giản nhất là sử dụng Windows bằng ** USBToolBox **.

##### Ánh xạ Cổng USB trong Windows (được khuyến nghị):
- Khởi động vào Windows
- Tải xuống phiên bản Windows của [** USBToolBox **](https://github.com/USBToolBox/tool/releases)
- Run it, follow the [**instructions**](https://github.com/USBToolBox/tool#usage) để ánh xạ các cổng USB của bạn
- Xuất `UTBMap.kext`
- Khởi động lại vào macOS, gắn EFI của bạn và thêm Kext vào thư mục `EFI \ CLOVER \ kexts \ Other` của bạn.
- Khởi động lại

Tận hưởng các cổng USB hoạt động bình thường của bạn!

##### Ánh xạ Cổng USB trong macOS 11.3 và mới hơn:
- Tải xuống phiên bản macOS của [** USBToolBox **](https://github.com/USBToolBox/tool/releases)
- Tải xuống phần bổ sung này [package](https://github.com/USBToolBox/kext/releases)
- Giải nén gói zip và thêm 2 kext đi kèm vào thư mục kext của bạn.
- Chạy USBToolBox và làm theo[**instructions**](https://github.com/USBToolBox/kext#usage) để ánh xạ các cổng USB của bạn.
- Tạo `UTBMap.kext` của bạn và thêm nó vào thư mục và cấu hình` EFI \ CLOVER \ kexts \ Other` của bạn.
- Xóa `UTBDefault.kext` khỏi thư mục Kexts.
- Khởi động lại.
Tận hưởng các cổng USB hoạt động bình thường của bạn!

### Phương pháp 2: Ánh xạ Cổng USB qua ACPI (Không phụ thuộc vào hệ điều hành. Chỉ được khuyến nghị cho người dùng nâng cao!)
Kể từ macOS Big Sur 11.3, Quirk `XHCIPortLimit` loại bỏ giới hạn cổng USB không còn hoạt động nữa. Điều này làm phức tạp quá trình tạo `USBPorts.kext` bằng các Công cụ như` Hackintool` hoặc `USBMap`.

Vì vậy, cách tốt nhất để khai báo các cổng USB là thông qua ACPI vì phương pháp này là OS-bất khả tri (không giống như kexts USBPort, theo mặc định chỉ hoạt động cho SMBIOS mà chúng được định nghĩa).

Làm theo [** hướng dẫn này **](https://github.com/5T33Z0/Clover-Crate/tree/main/USB_Fixes/ACPI_Mapping_USB_Ports) để ánh xạ các Cổng USB của bạn qua Bảng SSDT.
