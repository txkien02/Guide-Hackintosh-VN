# Aiko Guide Clover
[![Clover Version](https://img.shields.io/badge/Clover-r5143-lime.svg)](https://github.com/CloverHackyColor/CloverBootloader/releases)
[![Clover Configurator](https://img.shields.io/badge/Clover_Configurator-5.21.0-brightgreen.svg)](https://mackie100projects.altervista.org/download-clover-configurator/)
[![macOS](https://img.shields.io/badge/Supported_macOS-≤12.1-white.svg)](https://www.apple.com/macos/monterey/)
![Last Update](https://img.shields.io/badge/Last_Update_(yy.mm.dd):-21.12.30-blueviolet.svg)
![00Main](https://user-images.githubusercontent.com/76865553/136703368-146cda4c-9a8b-4b5f-8d3e-0382f1ccd68f.jpg)

## Thông Tin
Aiko Guide Clover là tài liệu không chính thức về Clover Bootloader và các tính năng của nó để giúp bạn tìm hiểu chức năng của từng tùy chọn. Cấu trúc của nó tuân theo thứ bậc của `config.plist` và các phần tương ứng trong Clover Configurator.

Hầu hết mỗi cài đặt / tham số có sẵn (bên cạnh những cài đặt rõ ràng) đều được giải thích. Phần ACPI đặc biệt chú ý đến từng chi tiết, vì đây là một trong những giao diện quan trọng nhất để cấu hình Hackintosh. Trong mỗi phần / chương, bạn có thể nhấp vào menu danh sách ở trên cùng bên trái để chọn một tính năng bạn quan tâm để chuyển đến mục nhập tương ứng, do đó bạn không phải cuộn hết tài liệu. Tôi đang nói về cái này:

![TOC](https://user-images.githubusercontent.com/76865553/136510478-2bccd5ae-6cc6-4a98-8f8d-63c41de2d3b3.png)

Tôi đã tạo repo này vì một số lý do:

1. **Overcoming** rào cản ngôn ngữ: Tài liệu chính thức của Clover (PDF, 171 trang) chỉ có sẵn bằng tiếng Nga.
2. **Condensing** thông tin có sẵn ở một nơi, trình bày thông tin đó theo kiểu mới hơn, sử dụng Markdown và github.
3. **Consolidating and Unifying** các nguồn nằm rải rác trên mạng: hướng dẫn sử dụng chỉ có sẵn bằng tiếng Nga trên github, changelog chỉ có tại insanelymac và chỉ được cập nhật thường xuyên. Wiki cỏ ba lá tại Source Forge đã không được cập nhật trong nhiều năm.
4. **Exemplifying** Các tính năng của Clover bằng cách sử dụng Clover Configurator.

<details>
<summary><strong>TUYÊN BỐ TỪ CHỐI</strong></summary>

| :warning: | ĐÂY KHÔNG PHẢI là Hướng dẫn Hackintosh! |
|-----------|:--------------------------------|

 Thông tin được cung cấp trong kho lưu trữ này chủ yếu dựa trên các đoạn trích của tài liệu tiếng Nga chính thức cho Clover r5129 bằng cách sử dụng các công cụ dịch dựa trên AI (deepl, google và yandex translate). Các bản dịch đã được xem xét và biên tập lại sau đó, để chúng tuân theo các quy tắc ngữ pháp và chính tả tiếng Việt trong khi vẫn giữ nguyên ý nghĩa của chúng. Tuy nhiên, một số dữ kiện có thể đã bị mất trong quá trình dịch.
</details>

## Mục lục
- **Clover Configuration**
  - [**ACPI**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/ACPI)
  - [**Boot**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/Boot)
  - [**Boot Graphics**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/Boot_Graphics)
  - [**CPU**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/CPU)
  - [**Devices**](https://github.com/herotbty/Guide-Hackintosh-Clo-/blob/main/Devices)
  - [**Graphics**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/Graphics)
  - [**GUI**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/GUI)
  - [**Kernel and Kext Patches**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/Kernel_And_Kext_Patches)
  - [**Quirks**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/Quirks)
  - [**RtVariables**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/RtVariables)
  - [**SMBIOS**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/SMBIOS)
  - [**System Parameters**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/System_Parameters)
- **Appendix**
  - [**Upgrading to Clover with OpenRuntime integration**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/Update_Clover)
  - [**Kext Management in Clover**](https://github.com/herotbty/Guide-Hackintosh-Clo-/blob/main/Kext_Management/README.md)
  - [**Mapping USB Ports**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/USB_Fixes)
  - [**Clover Laptop Configs and Hotpatches 2.0**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/Laptop_Configs)
  - [**Clover Dektop Configs**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/Desktop_Configs)
  - [**Converting OpenCore Properties to Clover**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/OC2Clover)
  - [**Clover Calculators**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/Xtras)
  - [**Utilities and Resources**](https://github.com/herotbty/Guide-Hackintosh-Clo-/tree/main/Utilities)

## ĐÓNG GÓP
Nếu bạn muốn đóng góp vào thông tin được cung cấp trong repo này để cải thiện / mở rộng nó, vui lòng tạo vấn đề với tiêu đề có ý nghĩa, liên kết đến chương / phần và mô tả những gì bạn muốn thêm, thay đổi, sửa hoặc mở rộng.

## CREDITS
- SergeySlice Cho [**Clover Bootloader**](https://github.com/CloverHackyColor/CloverBootloader)
- Acidanthera Cho [**OpenCore**](https://github.com/acidanthera/OpenCorePkg)
- Khronokernel Cho [**Clover Desktop Guide**](https://hackintosh.gitbook.io/r-hackintosh-vanilla-desktop-guide/)
- Dortania Cho the excellent Documentation on [**Fixing Post-Install issues**](https://dortania.github.io/OpenCore-Post-Install/) in particular 
- RehabMan Cho [**Clover Laptop Configs**](https://github.com/RehabMan/OS-X-Clover-Laptop-Config)
- Macki100Projects Cho [**Clover Configurator**](https://mackie100projects.altervista.org/download-clover-configurator/)
- daliansky Cho [**P-Little Repo**](https://github.com/daliansky/P-little)
- ic005k Cho [**PlistEDPlus**](https://github.com/ic005k/PlistEDPlus)
- uranusjr Cho [**MacDown**](https://macdown.uranusjr.com/)

