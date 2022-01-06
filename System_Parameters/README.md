# System Parameters
![sysparams](https://user-images.githubusercontent.com/76865553/136677062-ef979281-d50b-44a6-9b28-363c8cb70175.png)

## Độ sáng đèn nền
Mức độ sáng màn hình. Tuy nhiên, sẽ chỉ có một số hệ thống bị ảnh hưởng bởi tham số này. Nó cũng được đọc từ NVRAM. Theo mặc định, một giá trị do hệ thống cung cấp sẽ được sử dụng. Chỉ định nó trong tệp cấu hình sẽ ghi đè nó.

## CustomUUID
Số nhận dạng duy nhất của máy tính của bạn. Nếu bạn không nhập khóa này, một số thông tin phần cứng sẽ được tạo, nhưng nếu bạn muốn toàn quyền kiểm soát những gì xảy ra, hãy nhấn "Tạo mới" để tạo thông tin của riêng bạn hoặc "Lấy từ hệ thống".

## InjectKexts
Khóa này xác định chính sách toàn cầu liên quan đến việc chèn kext:

- `Yes`: Luôn tiêm kexts từ `/EFI/CLOVER/kexts/`
- `No`: Không bao giờ tiêm kexts

Kể từ Clover r5125, việc tải Kexts do OpenCore xử lý, vì vậy `FSInject` đã lỗi thời.

## InjectSystemID
Đối với người dùng Chameleon chuyển sang Clover !? - không được bảo hiểm ở đây, lỗi thời không quan tâm, để lại tàn tật và tiếp tục!

## NoCaches
Nếu được kích hoạt, bộ nhớ đệm sẽ bị bỏ qua trong mỗi lần khởi động. Và cũng giống như `InjectKexts`, khóa này xác định quy tắc chung, vì vậy giá trị được xác định trên Mục nhập tùy chỉnh sẽ ghi đè lên quy tắc này.

## NvidiaWeb
Nếu giá trị khóa là true, nó sẽ cho phép truy cập để tải và sử dụng kexts Nvidia WebDriver từ MacOS 10.12 trở đi. Tất nhiên, bạn vẫn cần cài đặt Nvidia Webdrivers, chỉ được hỗ trợ tối đa macOS High Siera (10.13.6).