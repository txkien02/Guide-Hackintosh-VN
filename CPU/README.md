# CPU
Phần này chứa các cài đặt để đặt các thông số cho CPU theo cách thủ công nếu các thuật toán bên trong không tự động phát hiện các thông số chính xác. Phần này chỉ được đề cập cho mục đích đầy đủ. Điểm mấu chốt là: ** bạn không thay đổi bất kỳ điều gì ở đây trừ khi điều đó không thể tránh khỏi và bạn biết mình đang làm gì! **

![CPU](https://user-images.githubusercontent.com/76865553/136802820-de658522-dad5-494c-b8a0-1362202f94ad.jpeg)

### Frequency MHz
Mô tả tần số cơ bản tính bằng MHz được hiển thị trong Hệ thống-Hồ sơ. Nói cách khác: tác dụng của nó hoàn toàn là mỹ phẩm. Thông thường, Clover tự động tính toán giá trị này dựa trên bộ đếm thời gian ACPI, nhưng nếu nó sai, bạn có thể sửa nó thông qua phím này.

### Bus Speed in kHz

Mô tả tần số xe buýt cơ sở bằng ** kHz **. Tần số bus rất quan trọng đối với hoạt động ổn định của hệ thống. Nó được chuyển từ bootloader sang kernel. Nếu tần số sai, hạt nhân sẽ không khởi động. Nếu tần số hơi tắt, có thể có vấn đề với đồng hồ, dẫn đến hành vi hệ thống lạ.

Bắt đầu với Clover r1060, tần số xe buýt được phát hiện tự động dựa trên dữ liệu từ Bộ hẹn giờ ADC tính toán các giá trị này chính xác hơn giá trị được lưu trữ trong DMI (Giao diện quản lý máy tính để bàn).

### Latency
Mô tả độ trễ để bật trạng thái C3. Giá trị tới hạn là ** 0x3E8 ** = ** 1000 **. Ít hơn và Speedstep bật, nhiều hơn và nó không bật. Trên máy Mac thực, nó luôn được đặt thành ** 0x03E9 ** (1001) để tắt Speedstep. Trên Hacks, chúng tôi có thể chọn nếu chúng tôi muốn xử lý nó giống như một máy Mac hoặc nếu chúng tôi muốn bật quản lý nguồn. Giá trị hợp lý cho giá trị sau là ** 0x00FA ** (250), được tìm thấy trên một số máy tính xách tay (MacPro5.1 = 17, MacPro6.1 = 67, iMac13.2 = 250).

### C2, C4, C6

Các thông số liên quan đến trạng thái C. ** CHÚ Ý! ** Phần này không được dùng nữa và từ đó đã được chuyển sang & rarr; [** ACPI **](https://github.com/5T33Z0/Clover-Crate/tree/main/ACPI#enable-c2-c4-c6-and-c7)

- `C2`: Đặt thành false trên máy tính hiện đại
- `C4`: Theo đặc điểm kỹ thuật, sử dụng C3 hoặc C4. Chúng tôi chọn C4. Đối với IvyBridge, hãy đặt nó thành false.
- `C6`: chỉ dành cho máy tính di động. Tuy nhiên, bạn cũng có thể thử kích hoạt nó trên máy tính để bàn. Đặt thành true trên IvyBridge và Haswell.

Lưu ý rằng với những C-States này, mọi người thường phàn nàn về các vấn đề với âm thanh / đồ họa và giấc ngủ. Vì vậy, hãy cẩn thận, hoặc loại trừ chúng hoàn toàn.

### QEMU
Khi thử nghiệm Clover trong máy ảo QEMU, các nhà phát triển đã phát hiện ra rằng nó không mô phỏng chính xác CPU Intel. Như một biện pháp tạm thời, khóa này đã được tạo ra.

### QPI
Trong Hồ sơ Hệ thống, giá trị này được gọi là "Tốc độ Bus của Bộ xử lý" hoặc đơn giản là "Tốc độ Bus". Đối với Clover, một thuật toán đã được phát triển để tính toán giá trị chính xác dựa trên bảng dữ liệu từ Intel. Trong mã nguồn của nhân `AppleSmbios`, có sẵn hai phương pháp đặt giá trị này: giá trị đã tồn tại trong SMBIOS, do nhà sản xuất quy định hoặc BusSpeed ​​x4 được tính toán đơn giản. Sau nhiều cuộc tranh luận, giá trị này đã được thêm vào cấu hình - viết những gì bạn thích (tính bằng MHz).

Điều này không ảnh hưởng đến công việc theo bất kỳ cách nào - đó là mỹ phẩm thuần túy. ** Theo thông tin mới nhất, QPI chỉ có ý nghĩa đối với các CPU thuộc dòng Nehalem **. Đối với những người khác ở đây, bạn cần có BusSpeed ​​x4. Hoặc không có gì cả. Nếu bạn buộc 0, thì bảng DMI 132 sẽ không được tạo ra.

### Type
Thông số này do Apple phát minh và được sử dụng trong cửa sổ "Giới thiệu về máy Mac này" để hiển thị thông tin về CPU đã sử dụng, thông số này được dịch nội bộ thành ký hiệu bộ xử lý. Nếu không, "Bộ xử lý không xác định" sẽ được hiển thị.

Về cơ bản, Clover biết tất cả các mật mã, nhưng vì tiến trình không đứng yên, nên có thể thay đổi giá trị này theo cách thủ công. Việc khắc phục điều này sẽ thay đổi những gì được hiển thị trong cửa sổ "Giới thiệu về máy Mac này" - hoàn toàn là mỹ phẩm. Có thông tin từ vit9696: [AppleSmBIOS](https://github.com/acidanthera/OpenCorePkg/blob/master/Include/Apple/IndustryStandard/AppleSmBios.h)

### TDP

Mô tả Công suất thiết kế nhiệt (tính bằng Watts), được tính đến ở các Bang khi tạo bảng Quản lý công suất bộ xử lý. Bạn có thể tìm thấy TDP cho CPU của mình trên [Trang web Thông số kỹ thuật sản phẩm của Intel](https://ark.intel.com/content/www/us/en/ark.html#@Processors)

### SavingMode
Một thông số thú vị khác để kiểm soát [Speedstep](https://en.wikipedia.org/wiki/SpeedStep). Nó ảnh hưởng đến thanh ghi MSR `0x1B0` và xác định hành vi của bộ xử lý:


- `0`: Hiệu suất tối đa
- `15 ': Tiết kiệm năng lượng tối đa

### HWPEnable
Bắt đầu với Clover r3879, công nghệ Intel Speed ​​Shift đã được giới thiệu trong các CPU Skylake. Nếu được bật, thì `1` được ghi vào thanh ghi MSR` 0x770`. Thật không may, nếu máy tính chuyển sang chế độ ngủ và sau đó thức dậy, giá trị MSR `0x770` sẽ được đặt lại thành` 0 'và Clover không thể bật lại nó. Nhưng với sự trợ giúp của [** HWPEnable.kext **](https://github.com/headkaze/HWPEnable) điều này có thể được sửa chữa.

### HWPValue
TGiá trị này hóa ra là thích hợp nhất. Giá trị của nó sẽ được ghi vào thanh ghi MSR `0x774`, nhưng chỉ khi MSR` 0x770` được đặt thành `1`. Nếu không, đăng ký này không có sẵn.

Các bộ vi xử lý thuộc dòng Intel Skylake có một tham số tần số cơ bản mới thay đổi theo từng bước nhỏ hơn tần số bus, cái gọi là `` ARTFrequency ''. Giá trị của nó thường là `24 MHz`. Clover có thể tính toán nó và cam kết nó vào lõi. Trong thực tế, tần số được tính toán dẫn đến hoạt động không chính xác, vì vậy nó có thể bị vô hiệu hóa một cách đơn giản. Trong trường hợp này, lõi hệ thống sẽ hoạt động theo cách riêng của nó. Trong các phiên bản mới hơn của Clover, con số này được làm tròn, vì vit9696 tin rằng chỉ có thể có ba giá trị và chúng làm tròn, lên đến `1 MHz`.

### TurboDisable
Tắt Công nghệ Intel Turbo Boost. Hữu ích cho Máy tính xách tay để chúng không quá nóng.