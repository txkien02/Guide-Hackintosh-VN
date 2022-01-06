
# SMBIOS
> ** SMBIOS ** là viết tắt của ** System Management BIOS **. Nó xác định cấu trúc dữ liệu (và phương pháp truy cập) có thể được sử dụng để đọc thông tin quản lý do BIOS của máy tính tạo ra. Điều này loại bỏ sự cần thiết của hệ điều hành để thăm dò phần cứng trực tiếp để phát hiện ra những thiết bị nào hiện diện trong máy tính. Đặc tả SMBIOS được sản xuất bởi Lực lượng Đặc nhiệm Quản lý Phân tán (DMTF), một tổ chức phát triển các tiêu chuẩn phi lợi nhuận. DMTF ước tính rằng hai tỷ hệ thống máy khách và máy chủ triển khai SMBIOS.
[** NGUỒN **: Wikipedia] (https://en.wikipedia.org/wiki/System_Management_BIOS#From_UEFI)

Phần này là cần thiết để bắt chước PC của bạn như một máy Mac. `ProductName` đã nhập có ý nghĩa đặc biệt ở đây. Nó không chỉ xác định phạm vi phiên bản macOS mà nó có thể chạy, hơn thế nữa, nó còn thiết lập rất nhiều thông số hệ thống cho phần cứng được sử dụng trong mô hình đã chọn. Vì vậy, việc chọn một mô hình gần với thông số kỹ thuật của phần cứng sẽ giúp bạn tiến gần hơn đến việc sở hữu một chiếc Hackintosh hoạt động tốt. Và điểm tham khảo quan trọng nhất để chọn một SMBIOS thích hợp là CPU bạn đang sử dụng.

Tôi sẽ không trình bày từng trường riêng lẻ của phần SMBIOS ở đây, vì nó tẻ nhạt và hoàn toàn không cần thiết, như bạn sẽ tìm hiểu ngay sau đây. Thay vào đó, tôi sẽ chỉ tập trung vào những cái quan trọng: `ProductName`,` SerialNumber`, `Board-ID` và` SmUUID`.

## Ví dụ: Tạo dữ liệu SMBIOS bằng Trình cấu hình Clover

Nếu bạn tìm thấy thư mục EFI cho Hệ thống của mình trên Diễn đàn hoặc trên Github, phần này có thể giống như sau:

![SMBIOS](https://user-images.githubusercontent.com/76865553/139639731-4eeb5cd4-9484-4477-ad0c-593c743293e0.png)
Như bạn có thể thấy, `ProductName` được đặt thành` MacBookPro11,4` (là Yêu cầu hệ thống tối thiểu để chạy macOS Monterey trên máy tính xách tay, btw). Bây giờ, hãy làm như sau để tạo Phần SMBIOS của riêng bạn:

1. Nhấp vào biểu tượng nhỏ này bên cạnh hộp kiểm "Chỉ cập nhật chương trình cơ sở". Cái này:

	
	![Dropdown](https://user-images.githubusercontent.com/76865553/136689944-182b5c46-ef9a-4495-bb4a-c9618cd1192c.png)

2. Tiếp theo, bạn sẽ phải đối mặt với một danh sách khổng lồ các mẫu máy Mac để lựa chọn:

	
	![models](https://user-images.githubusercontent.com/76865553/136689980-3d8739d2-5d22-4535-9c99-355b33191344.png)

3. Chọn kiểu máy Mac dựa trên dòng CPU Intel mà bạn đang sử dụng. Trong ví dụ này, mẫu MacBookPro thực tế sử dụng Intel i7 `` 4770HQ ''. Trong danh pháp của Intel, chữ số đầu tiên của tên kiểu CPU mô tả thế hệ (hoặc họ) mà nó thuộc về. Trong trường hợp này, đó là `4` cho CPU Intel" i "thế hệ thứ 4 (các kiểu máy thế hệ đầu tiên chỉ sử dụng 3 chữ số). Vì vậy, hãy chọn Mẫu máy Mac gần với mẫu của bạn về CPU (máy tính để bàn / thiết bị di động, thế hệ, tốc độ đồng hồ tối đa).
4. Sau khi bạn đã chọn một mô hình, Clover Configurator sẽ tự động tạo một SMBIOS hợp lệ cho bạn (không sao chép các giá trị này, hãy tạo của riêng bạn!):

	![SMBIOS_11,4](https://user-images.githubusercontent.com/76865553/139640510-0140ff1e-759b-4d75-846d-205db078197a.png)

	** QUAN TRỌNG **: Các trường được đánh dấu bằng màu lục lam * phải luôn bị xóa trước khi chia sẻ `config.plist` * của bạn. Ngoài ra, nếu bạn nhận được thông báo lỗi về chương trình cơ sở đã lỗi thời trong khi cài đặt macOS, hãy nhấp vào "Chỉ cập nhật chương trình cơ sở" và sau đó chọn lại cùng một kiểu máy Mac từ menu thả xuống. Điều này giữ Serial của bạn, v.v. và sẽ chỉ cập nhật thông tin phần sụn, vì vậy bạn có thể cài đặt macOS thành công.
5. Khi bạn đã tạo xong `SMBIOS` của mình, hãy chuyển đến phần` RTVariables`.
6. Sao chép số cho `MLB` (= Số sê-ri của bảng) được hiển thị trong cửa sổ" Thông tin "vào trường` MLB` và chọn "UseMacAddr0" từ menu thả xuống. Đây là điều bắt buộc để bật FaceTime.

 Done.

## Giới thiệu về "Chỉ cập nhật chương trình cơ sở"
Sử dụng tính năng này nếu cài đặt macOS không thành công do sự cố phần sụn. macOS kiểm tra xem yêu cầu về BIOS và Firmware của máy Mac thực có được đáp ứng hay không để đảm bảo rằng hệ điều hành sẽ hoạt động tốt. Nếu chương trình cơ sở đã lỗi thời, quá trình cài đặt sẽ dừng với một thông báo.

Bằng cách thiết lập BIOS và Firmwareversion mới nhất trong phần SMBIOS, rào cản này sẽ được khắc phục. Nó sẽ chỉ cập nhật phần BIOS và Phần sụn của SMBIOS mà không tạo ra một chuỗi mới:

- Chọn "Chỉ cập nhật chương trình cơ sở"
- Từ menu thả xuống bên phải, hãy chọn kiểu máy Mac của bạn (kiểu máy nằm dưới "Tên Procut") và nhấp vào mục nhập
- Cứu

Điều này cập nhật dữ liệu BIOS và chương trình cơ sở của bạn và bây giờ bạn có thể tiếp tục cài đặt macOS của mình.

## About "Extended Firmware Features"
![ExtFF](https://user-images.githubusercontent.com/76865553/139642095-73a2352b-c5e3-481b-8fc2-cdd6702a4dc1.png)
Mở rộng mặt nạ Tính năng chương trình cơ sở hiện có từ 32 bit lên tối đa 64 bit được yêu cầu cho các máy Mac và macOS Monterey mới nhất.

## LƯU Ý
Không giống như các máy Mac thực bị giới hạn ở một số phiên bản macOS được hỗ trợ nhất định, bạn có thể lừa macOS chạy trên các kiểu CPU mà nó không hỗ trợ chính thức - ít nhất, nếu SMBIOS được sử dụng không quá xa so với thông số kỹ thuật phần cứng của bạn.

Ví dụ: bạn có thể sử dụng SMBIOS dành cho CPU Haswell (Thế hệ thứ 4) với CPU IvyBridge (Thế hệ thứ 3), do đó mở rộng phạm vi phiên bản macOS mà bạn có thể chạy. Nhưng vì SMBIOS này được thiết kế cho một CPU khác, nó thực sự không hoạt động tốt, đặc biệt là trên Máy tính xách tay.

Cuối cùng, đó là một câu hỏi về điều gì quan trọng hơn đối với bạn: có thể chạy phiên bản macOS mới nhất chứ không phải tận dụng tối đa máy tính theo hiệu suất. Một giải pháp cho vấn đề này là sử dụng SMBIOS dành cho CPU của bạn nhưng thêm `-no_compat_check` vào các đối số khởi động. Nhược điểm là bạn không thể cài đặt các bản cập nhật hệ thống theo cách này, vì vậy bạn cần phải chuyển lại SMBIOS về bất kỳ thứ gì được macOS Monterey hỗ trợ chính thức để nhận các bản cập nhật và cũng có thể kích hoạt `HWTarget` trong RtVariables (sử dụng trình chỉnh sửa plist vì tham số này chưa khả dụng trong Clover Configurator).
