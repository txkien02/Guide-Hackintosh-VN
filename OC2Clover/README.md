# Converting OpenCore Properties to Clover
>Đang tiến hành! Vui lòng cung cấp thông tin bổ sung nếu bạn có và muốn đóng góp vào danh sách này.

Điều này dành cho những người dùng dự định chuyển đổi Cấu hình OpenCore đang hoạt động của họ sang Clover. Đây là bản chuyển thể của [** Hướng dẫn chuyển đổi cỏ ba lá **](https://github.com/dortania/OpenCore-Install-Guide/tree/master/clover-conversion) – jngược lại (và chuyên sâu hơn một chút).

Các phần phù hợp nhất để chuyển đổi cấu hình OpenCore thành Clover và ngược lại là: "ACPI", "Thuộc tính thiết bị", "Bản vá hạt nhân và Kext" và "Quirks", sẽ được đề cập ở đây.

## ACPI
Nói chung, bạn có thể sử dụng cùng một SSDT (.aml) trong Clover như trong OpenCore. Vì Clover không đưa Bảng ACPI vào Windows nên chúng không yêu cầu phương pháp if OSI Darwin. Một số SSDT được yêu cầu bởi OpenCore không cần thiết trong Clover vì chúng được bao gồm dưới dạng các bản sửa lỗi ACPI, được đưa vào trong quá trình khởi động.

### ACPI> Thêm
Sau đây là danh sách không đầy đủ các tệp .aml không cần thiết trong Clover, vì nó cung cấp các bản sửa lỗi DSDT cho chúng:

- `SSDT-SBUS-MCHC.aml` & rarr; Sử dụng các bản sửa lỗi`AddMCHC` và`FixSBUS`instead
- `SSDT-HPET.aml` → Sử dụng các bản sửa lỗi` FixHPET`, `FixIPIC`,` FixTMR` và `FixRTC` để thay thế. Bạn cũng không cần đổi tên kèm theo `change_CRS thành XCRS` được yêu cầu trong OpenCore.
- `DTGP` không phải là một phần của các bản vá SSDT. Nó sẽ được tạo bằng cách kích hoạt `AddDTGP`
-…

### ACPI> Xóa
Phần ACPI> Xóa có nghĩa là để xóa một số Bảng khỏi ACPI (thường được kết hợp với các SSDT khác để thay thế chúng, như SSDT-PM hoặc SSDT-PLUG chẳng hạn).

Trong Clover Configurator, phần này nằm trong ACPI> Drop Tables. Đây là các thông số có sẵn:
| OpenCore        | Clover              |
|:----------------|:--------------------|
| Table Signature | Signature           |
| OEMTableID      | Type/Key > TableID  |
| TableLength     | Type/Key > Length   |
| All             | DropForAllOS        |
| Enabled         | Disabled            |
| Comment         | –                 |

**Example**: Dropping CPU Power Management tables

- In OCAT:</br>
![OCAT_del](https://user-images.githubusercontent.com/76865553/138651510-a9d179a5-b8e3-43a4-9aff-3b536fffbbd7.png)

- In Clover Configurator:</br>
![Clover_drop](https://user-images.githubusercontent.com/76865553/138651565-3998b445-6df6-49b7-aad1-6a8914f169ef.png)

**NOTES**:

- In Clover, you enter actual names whereas in OpenCore you have to use HEX values.
- In Clover, you have to decide bewteen using either `TableID` or `TableLength`, not both.

Trong Clover, bạn nhập tên thực trong khi trong OpenCore, bạn phải sử dụng giá trị HEX.
- Trong Clover, bạn phải quyết định sử dụng `TableID` hoặc` TableLength`, không phải cả hai.
### ACPI > Patch
While Binary Renames work the same in OpenCore and Clover, you have a lot more options in OpenCore:

| OpenCore        | Clover    |
|:---------------:|:----------:|
| Comment         | Comment   |
| Find		        | Find      |
| Replace         | Replace   |
| Base            | use [TgtBridge](https://github.com/5T33Z0/Clover-Crate/tree/main/ACPI#tgtbridge) instead|
| Enabled         | Disabled  |
| Table Signature | –       |
| OEMTableID      | –       |  
| TableLength     | –       |
| Mask            | –       |
| ReplaceMask     | –       |
| Count           | –       |
| Limit           | –       | 
| Skip            | –       |
| BaseSkip        | –       |

In Clover you also have the option "RenameDevices" which works the same as the `Base` Parameter in OpenCore with the difference that it is limited to devices:</br>
![Ren_Dev](https://user-images.githubusercontent.com/76865553/138651675-a51e51df-8249-4a79-8d75-c9de401e268c.png)

In OCAT you enter the path to a device in the `base` field (don't forget to enable them):</br>
![Dev_base](https://user-images.githubusercontent.com/76865553/138652948-fc403674-e434-4980-8062-a1ae1e787ab6.png)

### ACPI > Quirks 

| OpenCore          | Clover      |
|:-----------------:|:-----------:|
| FadtEnableReset   | HaltEnabler |
| Normalize Headers | FixHeaders  |
| Reset Logo Status | Drop Tables > BGRT|
| Rebase Regions    | ?           |
| Reset HwSig			| ?           |
| SyncTableIDs      | ?           |

## DeviceProperties
In OCAT, devices are added or modified in a straight forward "DeviceProperties" section:</br>
![OCAT_Devprops](https://user-images.githubusercontent.com/76865553/138651900-29810b55-324d-48e8-8e97-fcdda6038612.png)

In Clover Configurator these are hidden bedind a button in a sub-section:</br>
![Devices](https://user-images.githubusercontent.com/76865553/138610651-c9d42248-d1b8-4e54-b16a-2819556ae60f.png)
Besides that they work the same and the data is interchangeable.

## Kernel
Dành cho người dùng muốn chuyển đổi Kext và Kernel Patch từ OpenCore sang Clover. Lưu ý rằng Clover sử dụng hai phần khác nhau để vá: một để vá Kexts ("KextsToPatch") và một để vá Kernel (KernelToPatch), trong khi trong OpenCore, điều này được xử lý trong cùng một phần ("Kernel> Patch").

### Kernel> Thêm
Trong Clover, bạn không phải thêm Kexts vào danh sách và bạn cũng không phải lo lắng về trình tự tải kext. Các phụ thuộc Kext được mã hóa cứng vào Clover, vì vậy Lilu và VirtualSMC sẽ luôn tải trước.

Trong khi trong OpenCore, bạn cần xác định kext nào tải cho phiên bản macOS nào thông qua tham số `Min / MaxKernel`, Clover xác định điều này thực tế hơn nhiều bằng cách sử dụng cấu trúc thư mục EFI bên trong` EFI \ CLOVER \ kexts \ `:

- Kexts sẽ tải cho * bất kỳ phiên bản * macOS nào phải được đặt trong `kexts \ other`. Đây là thư mục kexts chính (mặc dù tên không ngụ ý điều đó chút nào).
- Kexts chỉ nên tải cho các phiên bản macOS cụ thể, phải được đặt trong các thư mục tương ứng với phiên bản macOS sử dụng. Ví dụ: `kexts \ 10.15` cho Catalina,` kexts \ 11` cho Big Sur hoặc `kexts \ 12` cho macOS Monterey, v.v.
- Để vô hiệu hóa một Kext, hãy di chuyển nó đến `kexts \ Off`.

Điều này đỡ rườm rà hơn nhiều khi phải đặt giá trị `MinKernel` và` MaxKernel` cho Kexts. Mặt khác, nó làm cho việc cập nhật kexts phức tạp hơn một chút vì bạn phải tìm trong bất kỳ thư mục con nào. Thêm vào đó, có thể dẫn đến việc có một số kexts trùng lặp, do đó kích thước tổng thể của EFI tăng lên không ngừng.

### Kernel> Bản vá
Bên cạnh các mặt nạ `Find`/`Replace` cơ bản, có một số công cụ sửa đổi bổ sung mà bạn có thể sử dụng để áp dụng các bản vá lỗi Kernels và Kext. Trong bảng bên dưới, bạn tìm thấy các tùy chọn sẵn có và sự khác biệt về danh pháp giữa OpenCore và Clover:
| OpenCore    | Clover         | Description (where applicable) |
|:------------|:---------------|--------------------------------|
| Identifier  | Name           | 
| Base        | Procedure      | Name of the procedure we are looking for. The real name may be longer, but the comparison is done by a substring. Make sure the substring occurs only in "Procedure".
| Comment     | Comment        | Besides using it as a reminder what a patch does, this field is also used for listing the available patches in Clover's bootmenu.
| Find		    | Find           | value to find
| Replace     | Replace        | value to replace the found value by
| Mask        | MaskFind       |If some bit=1, we look for an exact match, if=0, we ignore the difference.
| ReplaceMask | MaskReplace    |If a bit=1, we make a replacement. If a bit=0, we leave it as is.
| –           | MaskStart      | Mask for the starting point, i.e. for the `StartPattern`. And then there are Find/MaskFind and Replace/MaskReplace pairs.
| –           | StartPattern   | Rement of the era before character patching was implemented. It marks the starting point from which to look for a replacement pattern. If we know the name of the procedure, `StartPattern` is hardly needed anymore.
| –           | RangeFind      | Length of codes to search. In general, just the size of this procedure, or less. This way we speed up the search without going through all the millions of strings.
| MinKernel   | –              | see "MatchOS"
| MaxKernel   | –              | see "MatchOS"
| Count       | Count          | number of time a replacement is made
| Limit       | –              | 
| Skip        | Skip           | nummber of time a match is skipped
| Enabled     | Disabled       | Disables the renaming rule (obviously)
| Arch        | –              | 
| –           | MatchOS        |Although there are no equivalents to `MinKernel` and `MaxKernel` parameters in Clover, you can use `MatcOS`. Instead of a range of kernel versions you just use the macOS version(s) it applies to. For example: `10.13,10.14,10.15` (without blanks in between).
| –           | MatchBuild     | no explanation available
| –           | InfoPlistPatch | no explanation available

## NVRAM > Add
### 7C436110-AB2A-4BBB-A880-FE41995C9F82
Từ UUID này, bạn có thể muốn chuyển:

- `boot-args`: Thêm những thứ này vào" Khởi động ">" Đối số "trong Clover
- `csr-active-config`: Cần được thêm vào" RtVariables ">" CsrActiveConfig ". NHƯNG bạn không thể sử dụng nó như hiện tại, vì nó là Endianness (hoặc bất cứ thứ gì nó được gọi) khác với hình thức mà Clover sử dụng. Ví dụ: `FF070000` là` 0x7FF` trong Clover. Bạn phải chuyển đổi nó:

1. Bỏ bốn chữ số cuối cùng (số không); bạn nhận được `FF07`
2. Chia 4 lần đào còn lại thành hai cặp: `FF` `07`
3. Hoán đổi vị trí của cặp thứ nhất và thứ hai; bạn nhận được:`07` `FF`
4. Bỏ dấu `0`; bạn nhận được: `7FF`
6. Nhập giá trị này vào "RtVariables"> `CsrActiveConfig`
7. Giá trị HEX kết quả sẽ giống như sau: `0x7FF`

** LƯU Ý **: Để tính bitmask csr của riêng bạn, bạn có thể sử dụng bảng tính [** Máy tính cỏ ba lá **] (https://github.com/5T33Z0/Clover-Crate/tree/main/Xtras) của tôi, bảng tính này cũng giúp hiểu toàn bộ choncept tốt hơn rất nhiều.

## Quirks
Đây là một trong những khía cạnh quan trọng nhất của cấu hình của bạn. Trong OpenCore có ACPI Quirks, Booter Quirks, Kernel Quirks và UEFI Quirks. Rất nhiều trong số chúng được kết hợp trong phần "Quirks" của Clover.

For more details on how OpenCore's Quirks are implemented into Clover, please refer to the Chapters [**"Quirks"**](https://github.com/5T33Z0/Clover-Crate/tree/main/Quirks) and [**Upgrading Clover for macOS 11+ compatibility**](https://github.com/5T33Z0/Clover-Crate/tree/main/Update_Clover).

### Kernel> Quirks
Mặc dù hầu hết các Bản vá lỗi hạt nhân của OpenCore đều nằm trong phần Quirks của Clover, nhưng thay vào đó, các bản vá lỗi sau lại nằm trong Bản vá lỗi hạt nhân và Kext:

| OpenCore (Kernel > Quirks)  | Clover (Kernel and Kext Patches)|
|:----------------------------|:--------------------------------|
| AppleCpuPmCfgLock           | AppleIntelCPUPM
| AppleXcpmCfgLock            | KernelPM
| AppleXcpmExtraMsrs          | KernelXCPM
| CustomSMBIOSGuid            | DellSMBIOSPatch
| DisableRtcChecksum          | AppleRTC
| LapicKernelPanic            | Kernel LAPIC
| PanicNoKextDump             | PanicNoKextDump

## PlatformInfo> Chung
Nếu bạn muốn sử dụng dữ liệu SMBIOS của mình từ "PlatformInfo> Generic" trong Clover, bạn phải sử dụng các trường và phần khác nhau để làm như vậy. Điều này có thể hơi khó hiểu do sự khác biệt về tên cũng như số lượng trường có sẵn trong cả hai cấu hình.

Để sử dụng dữ liệu SMBIOS trong OpenCore và Clover, hãy làm như sau:

1. Sao chép Dữ liệu từ các trường sau vào phần "SMBIOS" và "RtVariables" của Trình cấu hình Clover: </br>

	| OpenCore (PlatformInfo > Generic) | Clover (SMBIOS)      |
	|-----------------------------------|----------------------|
	| SystemProductName                 | ProductName          |
	| SystemUUID                        | SmUUID               |
	| ROM                               | ROM (in RtVariables) |
	| N/A in "Generic"                  | Board-ID             |
	| SystemSerialNumber                | Serial Number        |
	| MLB                               | Board Serial Number and MLB (in RtVariables)|
2. Tiếp theo, đánh dấu vào ô "Chỉ cập nhật chương trình cơ sở".
3. Từ Menu thả xuống bên cạnh, chọn cùng một Model mà bạn đã sử dụng cho "ProductName". Điều này cập nhật các trường khác như BIOS và Firmware.
4. Lưu cấu hình và khởi động qua Clover.

**QUAN TRỌNG**

- Nếu bạn đã làm đúng mọi thứ, bạn sẽ không phải nhập Mật khẩu AppleID của mình sau khi chuyển đổi bộ nạp khởi động và macOS sẽ cho bạn biết rằng "AppleID này hiện được sử dụng với thiết bị này" hoặc đại loại như vậy.
- Nhưng nếu macOS yêu cầu mật khẩu AppleID Password và mật khẩu Mail của bạn, v.v. sau khi chuyển đổi tải khởi động, bạn đã làm sai. Trong casem này, bạn nên khởi động lại vào OpenCore và kiểm tra lại. Nếu không, bạn đang đăng ký máy tính của mình dưới dạng máy Mac mới / khác.