# GUI
This section is for configuring the look and behavior of Clover's Boot menu GUI as well as defining which Volumes are displayed.

![GUI](https://user-images.githubusercontent.com/76865553/136703745-7ec35d11-5458-482c-a8c8-ccaf48d9650d.jpeg)

#### CustomIcons
Bật khóa này sẽ sử dụng các biểu tượng đĩa tùy chỉnh được lưu trữ trong thư mục gốc của chính ổ đĩa, thay vì sử dụng biểu tượng đĩa từ một chủ đề. Đó là tệp này: `.VolumeIcon.icns` là một tệp ẩn. Để hiển thị tất cả các tệp trong Finder, hãy sử dụng:

`mặc định ghi com.apple.finder AppleShowAllFiles TRUE && killall Finder`

Sau khi bạn thực hiện xong các thay đổi của mình, hãy đặt lại thành `FALSE`:

`mặc định ghi com.apple.finder AppleShowAllFiles FALSE && killall Finder`

### Theme
Nhập tên của một chủ đề được cài đặt trong `EFI \ Clover \ themes \ ThemeXYZ` chẳng hạn. Sử dụng tên thư mục để tham khảo. Trong ví dụ này, hãy nhập "ThemeXYZ".


### EmbeddedThemeType
Chọn giữa biến thể `Dark` và` Light` của một chủ đề được nhúng. Nếu không được đặt, các biến thể được chọn dựa trên đồng hồ thời gian thực - sáng vào ban ngày, tối vào ban đêm (nếu chủ đề này hỗ trợ). Được giới thiệu trong r4773.

Clover r4773 giới thiệu khái niệm thay đổi phong cách chủ đề của các chủ đề nhúng tùy thuộc vào thời gian trong ngày. Từ 8:00 sáng đến 8:00 tối, một biến thể sáng sẽ được sử dụng và từ 8:00 tối đến 8:00 sáng, biến thể tối sẽ được sử dụng. Nó dựa trên thời gian GMT, vì vậy hãy cộng hoặc trừ số giờ cho vị trí của bạn.

### KbdPrevLang
Chọn Bố cục Bàn phím ưa thích của bạn cho macOS nếu bạn muốn lưu ngôn ngữ hệ thống khi nâng cấp macOS với NVRAM gốc.

Kết hợp với `Ngôn ngữ`, điều này có thể được sử dụng để khắc phục sự cố khi cài đặt phiên bản beta của macOS. Những ngôn ngữ này đôi khi không bao gồm các ngôn ngữ khác ngoài tiếng Anh. Trong trường hợp này, bạn sẽ chỉ thấy một màn hình màu xám nơi có trợ lý cài đặt. Chỉ cần đặt `Language` thành` en-US: 0` và bạn sẽ ổn.

### Ngôn ngữ
Hiện tại, việc đặt ngôn ngữ chỉ có ý nghĩa đối với menu "Trợ giúp" được gọi bằng Phím F1. Tuy nhiên, giá trị này được gửi đến hệ thống và có thể ảnh hưởng đến ngôn ngữ mặc định.

### ScreenResolution
Tại đây bạn có thể thay đổi Độ phân giải màn hình mặc định của GUI menu Boot. Giá trị mặc định là 1024x768 px. Clover phát hiện độ phân giải cao nhất có thể, nhưng nó có thể sai, vì vậy bạn có thể chọn giá trị chính xác hoặc mong muốn từ menu thả xuống.

### Chế độ Bảng điều khiển
Giá trị chính xác có thể được tìm thấy trong `boot.log` của bạn hoặc đặt nó thành` Max` và độ phân giải tối đa có thể sẽ được sử dụng cho chế độ bảng điều khiển.

### PlayAsync
Clover r4833 đã thêm hỗ trợ âm thanh cho menu Khởi động thông qua trình điều khiển `AudioDxe.efi`. `PlayAsync` xác định chế độ phát lại của chuông khởi động: tuần tự hoặc đồng thời. Với tính năng phát lại đồng bộ (mặc định), quá trình khởi động diễn ra tuần tự: chuông báo trước, sau đó trình khởi động khởi động. Nếu bật `PlayAsync`, quá trình khởi động sẽ chạy song song hoặc đồng thời với quá trình phát lại âm thanh. Chuông phải được đặt tên là `sound.wav` cho chủ đề ban ngày và` sound_night.wav` cho chế độ ban đêm và cần được đặt trong thư mục gốc của chủ đề đã sử dụng.

**NOTES**: 

- Nếu tệp quá dài, tệp sẽ bị cắt khi macOS kiểm soát trình điều khiển âm thanh.
- Theo ý kiến ​​của tôi (5T33Z0), tính năng này nên được gọi là `` PlaySimult` ', vì đó là những gì nó làm. Sử dụng thuật ngữ không đồng bộ trong ngữ cảnh âm thanh gợi lên cảm giác về các vấn đề âm thanh hơn là một tính năng.

### ProvideConsoleGop
Tạo giao thức GOP cho chế độ bảng điều khiển, tức là cho đầu ra văn bản không phải ở chế độ văn bản, như bạn đã quen làm trong PC BIOS, nhưng ở chế độ đồ họa, như Apple đã làm.

Trong các bản sửa đổi trước r5128, cài đặt này cũng có trong Quirks với tên gọi là `SupplyConsoleGopEnable` nhưng sau đó đã bị loại bỏ để tránh các tham số trùng lặp.

`Cung cấpConsoleGop` từ GUI sẽ ghi đè 'Cung cấpConsoleGopEnable` từ phần` Quirks` - đề phòng trường hợp bạn quên xóa tham số này khỏi cấu hình khi cập nhật Clover.

### ShowOptimus
Bật thông báo trên màn hình trong menu Khởi động về GPU nào được bật, vì vậy bạn có thể tắt GPU Optimus rời rạc ngay lập tức vì chúng không được macOS hỗ trợ. Nếu GPU trên bo mạch và GPU rời được bật, thông báo `` Intel Discrete` sẽ hiển thị.

### TextOnly
Chế độ menu chỉ văn bản cho GUI tối thiểu và thời gian tải nhanh hơn (giống như năm 1984).

## Con chuột
### Đã bật
`Enabled` - tự giải thích. Tắt chuột nếu nó gây ra sự cố trong menu Khởi động

### Tốc độ, vận tốc
`Speed` - Đặt tốc độ của con trỏ, các giá trị hợp lý là 2 đến 8. Một số chuột yêu cầu tốc độ âm, di chuyển theo hướng ngược lại. Giá trị 0 có nghĩa là chuột bị vô hiệu hóa.

### Gương
Đảo ngược tốc độ chuột để sửa chuyển động chuột bị đảo ngược.

## Scan
![GUI_Scan](https://user-images.githubusercontent.com/76865553/136699885-281deddd-0151-4e06-a489-4299669fe4d3.png)

Trong phần phụ này, bạn có thể sửa đổi các tính năng quét của Clover cho đĩa. Nói chung, bạn chọn càng ít tùy chọn, giao diện người dùng xuất hiện càng nhanh. Khi được đặt thành `Tự động`, Clover quyết định phải làm gì. Nếu bạn đặt nó thành `Custom`, bạn có quyền kiểm soát các tính năng sau:


### Entries
Tùy chọn này bật hoặc tắt tính năng quét các bản ghi UEFI trên mỗi đĩa khi khởi động. Nó cũng áp dụng các quy tắc được đặt trong phần "Ẩn số lượng" và "Mục nhập tùy chỉnh".

### Legacy
Tham số này bật hoặc tắt tính năng tìm kiếm bộ nạp khởi động cũ được khởi chạy từ PBR. Điều này có thể được tinh chỉnh bằng cách chọn các tiêu chí bổ sung từ menu thả xuống: "Đầu tiên" đặt các phần kế thừa ở đầu danh sách, "Cuối cùng" đặt chúng ở cuối danh sách. Không ai sử dụng cái này bao giờ, vì vậy chúng ta hãy tiếp tục.

### Tool
Cài đặt này bật hoặc tắt tính năng tìm kiếm các công cụ UEFI trên mỗi phân vùng khi Clover khởi động.

### Linux
Tùy chọn này bật hoặc tắt tính năng tìm kiếm bộ nạp khởi động Linux trên mỗi phân vùng khi Clover khởi động. Sử dụng kết hợp với các tùy chọn "Kernel". Không nên quét các bộ nạp khởi động Linux trên từng phân vùng, rất mất thời gian!

### Kernel
Tham số này bật hoặc tắt tính năng tìm kiếm hạt nhân Linux trên mỗi phân vùng. Điều này có thể được tinh chỉnh bằng cách chọn các tiêu chí bổ sung từ menu thả xuống (xem ảnh chụp màn hình): "Đầu tiên" đặt các phần kế thừa ở đầu danh sách, "Cuối cùng" đặt các phần kế thừa ở cuối danh sách, v.v. Không ai sử dụng cái này bao giờ, vì vậy chúng ta hãy tiếp tục.

Tham số này bật hoặc tắt tính năng tìm kiếm bộ nạp khởi động cũ được khởi chạy từ PBR dựa trên phiên bản hạt nhân.

## Custom Entries
Trong phần phụ này, bạn có thể thêm các mục của riêng mình vào GUI menu Khởi động. Điều này rất hữu ích nếu một mục nhập bị thiếu hoặc nếu bạn muốn tùy chỉnh nó. Nhấp vào biểu tượng "+" để thêm một mục vào danh sách. Sau khi nhấp đúp vào mục nhập, một menu phụ sẽ được mở ra:


![Custom_Entry](https://user-images.githubusercontent.com/76865553/136745225-5c06bd82-b7a8-4459-b29d-52870742941e.png)

Ở đây bạn có thể thay đổi rất nhiều thứ. Điều quan trọng nhất là menu thả xuống để chọn "Âm lượng" mà mục nhập tùy chỉnh phải đại diện:

![GUID](https://user-images.githubusercontent.com/76865553/136699942-79efe2a9-2995-48fe-a6fd-aad342ae259a.png)

Nhập tên tùy chỉnh vào trường `Title`, đặt` Type` và `Volume Type` và nó sẽ hoạt động.

## Ẩn Âm lượng
Tại đây, bạn có thể sử dụng nhập tên của các phân vùng / khối lượng mà sẽ được ẩn khỏi GUI Bootloader theo mặc định. Đối với một số khối lượng Windows nhất định, bạn cần nhập UUID chính xác để ẩn chúng. Mở Terminal và nhập:

1. `diskutil list` và nhấn enter
2. Tìm phân vùng bạn muốn ẩn
3. Sao chép Định danh của nó ("diskXsY") vào khay nhớ tạm thời hoặc lưu trữ trong một tệp văn bản.
4. Tiếp theo, nhập `diskutil info diskXsY | grep -i "UUID phân vùng" | phiên bản | cắt -d '' -f 1 | vòng quay`. Thay thế diskXsY bằng số nhận dạng thực của bạn và nhấn enter
5. Sao chép `UUID`
6. Trong phần "Ẩn âm lượng", hãy nhấp vào dấu `+`
7. Nhập UUID

Vào lần khởi động lại tiếp theo, tập này sẽ bị ẩn.

** Mẹo **: Một phương pháp dễ dàng hơn để tìm ra UUID là sử dụng phần mục nhập tùy chỉnh, vì nó hiển thị UUID của tất cả các tập.