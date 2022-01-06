# Kernel and Kext Patches
![KernelQuirks](https://user-images.githubusercontent.com/76865553/136670474-678b7ae1-b5ec-4791-963a-7af091a833ca.png)

Đây là một nhóm các tham số để tạo các bản vá nhị phân một cách nhanh chóng. Lưu ý rằng điều này chỉ có thể được thực hiện nếu kernelcache hoặc tham số `ForceKextsToLoad` được tải. Nếu kext không được tải và không có trong bộ nhớ cache, các bản vá này sẽ không hoạt động. Phần này bao gồm 2 danh mục: một có Kernel Patches mà bạn có thể nhấp vào để bật. Chúng chủ yếu để làm cho CPU của bạn hoạt động với macOS. Loại thứ hai là để tạo các bản vá lỗi của riêng bạn, có thể được áp dụng cho kexts và kernel.

## ATI Section
Phần này áp dụng cho macOS 10.7 trở lên sử dụng Card đồ họa ATI / AMD. Có 3 trường mà dữ liệu phải được nhập để thẻ và các đầu nối của nó hoạt động hoàn toàn trong macOS:

- `ATIConnectorsController`: tại đây bạn nhập mã định danh của thẻ. Trong ví dụ này, chúng tôi sử dụng AMD Radeon 6000.
- `ATIConnectorsData` và` ATIConnectorsPatch` yêu cầu dữ liệu phải được tính toán để cung cấp cho macOS thông tin về (các) đầu ra và (các) đầu nối của GPU.

Để tìm hiểu cách tính các giá trị này, bạn có thể đọc [** chủ đề **] này (https://www.insanelymac.com/forum/topic/249642-editing-custom-personalities-for-ati-radeon-hd45xx/) b
bởi bcc9 (tiếng anh). Đối với Radeon, RX, Vega hiện tại và các thẻ khác, bạn có thể làm theo [** hướng dẫn **] này(http://www.applelife.ru/threads/Завод-ati-hd-6xxx-5xxx-4xxx.28890/) by Xmedik (russian).

### ATIConnectorsController
Để chạy đầy đủ các thẻ ATI / AMD Radeon 5000 và 6000 series, việc tiêm các thuộc tính của thiết bị là chưa đủ, bạn còn phải điều chỉnh các đầu nối trong bộ điều khiển tương ứng.
Trong ví dụ này, chúng tôi vá bộ điều khiển của AMD Radeon 6000, vì vậy chúng tôi nhập `6000`. Tiếp theo, các giá trị cho `ATIConnectorsData` và` ATIConnectorsPatch` phải được cung cấp.

### ATIConnectorsData
Dữ liệu Trình kết nối phải được cung cấp dưới dạng `<string>`. Trong ví dụ này đối với thẻ dòng 6000, giá trị là:00040000040300000001000021030204040000001402000000010000000004031000000010000000
0001000000000001`

### ATIConnectorsPatch
Dữ liệu bản vá của trình kết nối phải được cung cấp dưới dạng `<string>`. Trong ví dụ này đối với thẻ dòng 6000, giá trị là:04000000140200000001000000000404000400000403000000010000110201050000000000000000
0000000000000000`

**NOTE**: Trong macOS 10.12, các trình kết nối được sử dụng trong ví dụ này sẽ khác nhau, vì vậy dữ liệu được sử dụng trong ví dụ này không mang tính đại diện, mặc dù phương pháp tính giá trị chính xác cho `ATIConnectorsData` và` ATIConnectorsPatch` vẫn giống nhau.

## Bản vá hạt nhân
Các hộp này phải được chọn khi thiết lập các câu hỏi thường gặp & rarr; [**Quirks**](https://github.com/5T33Z0/Clover-Crate/tree/main/Quirks) 
cho CPU đã sử dụng. Thông thường, bạn cần `AppleIntelCPUPM` cho các CPU Intel cũ hơn (lên đến Ivy Bridge) hoặc` KernelPM` (Haswell và mới hơn).

### AppleIntelCPUPM
& rarr; Xem [**Quirks**](https://github.com/5T33Z0/Clover-Crate/tree/main/Quirks) Section

### AppleRTC
Đã lỗi thời! vit9696 đã điều tra sự cố và sửa các hoạt động RTC trong Clover, hiện giá trị khóa được đề xuất là `false` vì nó ảnh hưởng đến chế độ ngủ đông.

### Gỡ lỗi
Nếu bạn muốn quan sát cách các Kexts được vá & rarr; Cho các nhà phát triển.

### DellSMBIOSPatch
& rarr; Xem [** Quirks **](https://github.com/5T33Z0/Clover-Crate/tree/main/Quirks) Section

### FakeCPUID

Gán một ID thiết bị khác cho CPU đã sử dụng. Hữu ích khi cố gắng chạy phiên bản macOS cũ hơn với CPU mới hơn không được hỗ trợ. Ví dụ: nếu bạn đang cố gắng chạy macOS High Sierra hoặc Mojave với CPU Comet Lake không được hỗ trợ trước macOS 10.15, bạn có thể sử dụng Device-ID của họ CPU Coffee Lake để macOS chấp nhận nó.

### EightApple
Trên một số hệ thống, thanh tiến trình chia thành 8 quả táo trong khi khởi động. Chưa có xác nhận nếu bản vá hoạt động. Đã thêm vào r5119.
### KernelLapic
&rarr; See [**Quirks**](https://github.com/5T33Z0/Clover-Crate/tree/main/Quirks) Section

### KernelPm
&rarr; See [**Quirks**](https://github.com/5T33Z0/Clover-Crate/tree/main/Quirks) Section

### KernelXCPM
&rarr; See [**Quirks**](https://github.com/5T33Z0/Clover-Crate/tree/main/Quirks) Section

## About Patching Masks
![KnKPatches](https://user-images.githubusercontent.com/76865553/136670510-106715c6-884d-4e6a-b151-34d45d9b231a.png)


Bắt đầu với r5095, khả năng tạo các bản vá nhị phân theo quy tắc đổi tên với mặt nạ vá phức tạp đã được giới thiệu cho những điều sau:
- `KextPatches`
- `KernelPatches` 
- `BootPatches`

Bên cạnh các mặt nạ `Find` /` Replace` cơ bản, có một số công cụ sửa đổi bổ sung mà bạn có thể sử dụng để áp dụng các bản vá lỗi Kernels và Kext. Trong bảng bên dưới, bạn tìm thấy các tùy chọn sẵn có và sự khác biệt về danh pháp giữa OpenCore và Clover:

| OpenCore    | Clover         | Description (where applicable) |
|:-----------:|:--------------:|--------------------------------|
| Identifier  | Name           | Name of the Kext/Kernel the patch should be applied to.
| Base        | Procedure      | Name of the procedure we are looking for. The real name may be longer, but the comparison is done by a substring. Make sure the substring occurs only in "Procedure".
| Comment     | Comment        | Besides using it as a reminder what a patch does, this field is also used for listing the available patches in Clover's bootmenu.
| Find		    | Find           | Value to find
| Replace     | Replace        | Value to replace the found value by
| Mask        | MaskFind       | If some bit=1, we look for an exact match, if the bit=0, we ignore the difference.
| ReplaceMask | MaskReplace    | If a bit=1, we make a replacement. If a bit=0, we leave it as is.
| –           | MaskStart      | Mask for the starting point, i.e. for the `StartPattern`. And then there are Find/MaskFind and Replace/MaskReplace pairs.
| –           | StartPattern   | Remnent of a time before character patching was implemented. It marks the starting point from which to look for a replacement pattern. If we know the name of the procedure, `StartPattern` is hardly needed anymore.
| –           | RangeFind      | Length of code to search. In general, just the size of this procedure, or less. This speeds up the search query without going through all the millions of strings.
| MinKernel   | –              | &rarr; see "MatchOS"
| MaxKernel   | –              | &rarr; see "MatchOS"
| Count       | Count          | Number of times a replacement is made
| Limit       | –              | 
| Skip        | Skip           | Number of times a match is skipped
| Enabled     | Disabled       | Disables the renaming rule (obviously)
| Arch        | –              | 
| –           | MatchOS        |Although there is no equivalent to `MinKernel` and `MaxKernel` in Clover, you can use `MatchOS` to limit a patch to specific versions of macOS. But instead of setting a range of Darwin kernels, you just set the macOS version(s) it applies to. For example: `10.13,10.14,10.15` (without blanks). You can also use masked strings like `11.5.x` (= for all 11.5 and sub-sequent variants, like 11.5.4), `12.x` (= for all variants of macOS 12), etc. 
| –           | MatchBuild     | Applies/Limits a patch to a specific system build of macOS, such as `21D5025F`, for example. You can list multiple builds separated by commas. If no value for `MatchBuild` is set, the patch applies to all builds. In general, patching kexts or kernels based on the build version is not very common and rarely needed.
| –           | InfoPlistPatch | Applies patches to parameters inside of `plists` of kexts defined in the `Name` field. The search can include multiple strings, excluding all invisible characters, such as line feeds and tabs. The search should be set as `<data>`, because the service characters such as "<" can not be set in text form. The lengths of the search and replacement strings can be different, but need to have the same length (fill one mask with spaces to match the length of the other one if necessary).

### KextsToPatch
Đây là phần thường được sử dụng để vá kexts nhằm kích hoạt các tính năng như Trim hoặc sử dụng các bản vá cổng USB để sử dụng nhiều hơn 15 cổng cho mỗi Bộ điều khiển (không còn bắt buộc vì bạn có thể sử dụng Quirk `XhciPortlimit` cho điều đó ngay bây giờ).

Nguyên tắc vá tương tự như nguyên tắc được sử dụng để vá `DSDT`, nhưng thay vào đó bạn sẽ vá những thứ bên trong kexts. Bạn nhập tên của Kext mà bạn muốn vá và sau đó nhập giá trị cỏ ba lá cần tìm và thay thế. Kiểm tra menu thả xuống để tìm nhiều bản vá có thể hữu ích.

### KernelToPatch
Để áp dụng các bản vá cho Nhân Darwin của macOS. Điều này chủ yếu được các nhà phát triển sử dụng để vá các nhân hoặc cho mục đích gỡ lỗi. Trong một số trường hợp hiếm hoi, nó được sử dụng để bật các tính năng không hoạt động khác, như bật XCPM trên CPU IvyBridge hoặc bật Bộ điều khiển Ethernet Intel I-225 trong macOS Catalina.

### BootPatches
Để áp dụng các bản vá nhị phân cho `boot.efi`