# _Disk Analysis_

![image](https://github.com/user-attachments/assets/40696e30-7945-409c-9d6a-84f2a54bfc83)

Theo thói quen check trong `console history` thì có ngay được flag

![Screenshot 2025-05-25 212846](https://github.com/user-attachments/assets/37994f95-50a5-4f22-8f19-00e32e0752e7)

Truy cập ảnh

![Screenshot 2025-05-25 212927](https://github.com/user-attachments/assets/c42cfc7e-3228-4564-9a7f-f4e9ea280534)

# _Noted_

![image](https://github.com/user-attachments/assets/76f68cbb-65cd-47f4-b023-09eb76afe02b)

Bài cho file .img, theo tiêu đề có ghi là `Windows 11 remembers everything, even when it's not saved.` và tên bài `Noted` nên mình nghĩ đến tính năng  `auto-save` của `Notepad`

Tiến hành check theo path `C:\Users\\AppData\Local\Packages\Microsoft.WindowsNotepad_8wekyb3d8bbwe\LocalState\TabState`

![image](https://github.com/user-attachments/assets/6ae4d803-4d5e-4749-bd54-25fcdd2408c1)

Save all về và xem từng file là lấy được flag

![image](https://github.com/user-attachments/assets/96a8782d-6b8c-46bb-b9b6-330f99c35da5)

# _Volinux_

![image](https://github.com/user-attachments/assets/dced2296-4fb6-4600-983d-a03847c5fd2e)

Bài này cho 1 file memdump của linux và có giới thiệu đến 1 tool do chính họ đang phát triển là `volinux`, cốt lõi là nó dựa trên volatility3 nhưng dạng GUI

![image](https://github.com/user-attachments/assets/a0e2a48b-bf52-4241-89b8-090d3be8d8e9)

Có được tool này cũng hay tại nó sẽ tự động nhận diện nên sẽ bỏ qua được bước build kernel, symbol... thủ công

![image](https://github.com/user-attachments/assets/22af65f0-ae2c-4165-8401-dc471148bdeb)

Làm theo hướng dẫn là mình sẽ truy cập được web, drop file vô rồi tiến hành phân tích

![image](https://github.com/user-attachments/assets/d7c6f897-72db-4d71-b469-efd29ff62514)

>Q1. What is the Linux kernel version of this memory dump?

![image](https://github.com/user-attachments/assets/19d9dd63-5fe6-4bfd-b7b6-4f789643c134)

Cái này tương ứng với `banners.Banners`

`Answer: 4.15.0-112-generic`

>Q2. What was the command used to run website on the machine?

Câu này mình sẽ đi chọn plugin `Bash history` tương ứng với `linux.bash`

![image](https://github.com/user-attachments/assets/da62de3f-f0e7-4c59-8272-4f3cd4e0e7f4)

Có thể thấy được hành vi của attacker là dựng máy chủ PHP, cấp quyền,... sau đó khởi chạy server cục bộ bằng lệnh `php -S localhost:8080`

`Answer: php -S localhost:8080`

>Q4. What is the IP address of the machine?

Chọn plugin `IP address` tương ứng với `linux.ip.Addr`

![image](https://github.com/user-attachments/assets/a2c8205a-3155-4381-9c31-d3cc1975fcf5)

`Answer: 192.168.1.130`

>Q5. What is the MAC address of enp0s8 interface?

Vẫn trong hình trên ta có được ngay địa chỉ MAC

`Answer: 08:00:27:e5:4e:b8`

>Q6. When was the linux session started?

Dùng `Bootime info` tương ứng với `linux.boottime.Boottime`

![image](https://github.com/user-attachments/assets/2d9527a2-8492-49ae-b42c-0c98e66729b5)

`Answer: 2025-04-21 16:20:46.752644 UTC`

>Q8. What's the name of parent process of php process?

![image](https://github.com/user-attachments/assets/a8c8ff47-8835-4c76-bed1-39e83622bc3f)

PID 1227 là tiến trình cha của `php`, lướt lên xíu để tìm tên

![image](https://github.com/user-attachments/assets/de493839-a0be-4fc5-bbda-629965a0aaf6)

`Answer: upstart`

>Q7. An attacker has uploaded a file to the machine. What is the inode address of the file?

Dùng plugin `List File in Memory` tương đương với `linux.pagecache.Files`

![image](https://github.com/user-attachments/assets/af865203-c321-444f-ac4d-1d0efbed819a)

Vì mình đã check qua `bash history` thì thấy được một lệnh `php -S localhost:8080` đã được chạy. Khi chạy thì PHP sẽ tự động dùng thư mục hiện tại làm web root, cho phép truy cập hay upload các file .php

Nên mình sẽ tìm theo extension .php thì tìm thấy 1 file với tên `CVEugeneDELACROIX.pdf.php` trông rất khả nghi

`backend_1   | 0x9f351d64d000    /       8:1     392545  0x9f35145b2770  REG     3       0       -rw-r--r--      2025-04-21 16:25:03.071470 UTC 2025-04-21 16:24:52.346110 UTC  2025-04-21 16:24:52.434154 UTC  /var/www/unrestricted-file-upload-exercise-main/victim-service/uploads/CVEugeneDELACROIX.pdf.php`  

Thứ tự các mốc thời gian kịch bản của attacker như sau:

| Thời gian (UTC)             | Hành động                                                                      | Nguồn dữ liệu                |
|-----------------------------|----------------------------------------------------------------------------------|------------------------------|
| 2025-04-21 16:21:39         | Khởi chạy server PHP lần đầu với `php -S localhost:8080`                        | `linux.bash` (PID 2145)      |
| 2025-04-21 16:24:04         | Server PHP được khởi chạy lại (có thể là restart để cập nhật mã nguồn)         | `linux.bash` (PID 2145)      |
| 2025-04-21 16:24:52.346110  | **File `CVEugeneDELACROIX.pdf.php` được upload/lưu (ModificationTime)**        | `linux.pagecache.Files`      |
| 2025-04-21 16:25:03.071470  | **File `CVEugeneDELACROIX.pdf.php` được truy cập/thực thi (AccessTime)**       | `linux.pagecache.Files`      |

`Answer: 0x9f35145b2770`

>Q3. From which path the php command was executed?

Như phân tích ở trên, nếu chỉ nhìn vào `Bash history` thì chưa thể biết chính xác path mà attacker đang đứng. Do server được khởi chạy trước khi `upload` nên path mà attacker đang đứng sẽ lùi lại 1 thư mục so với vị trí của file `CVEugeneDELACROIX.pdf.php`

`Answer: /var/www/unrestricted-file-upload-exercise-main/victim-service`

![image](https://github.com/user-attachments/assets/f9fd1432-eaf0-46e3-bfa8-7a6106f208ef)

Nối chúng lại theo đúng format và tính md5

![image](https://github.com/user-attachments/assets/aec69b82-ed95-4ba7-a72f-03f2445bc55b)

`Flag: DVCTF{d0853c14865ee8562983c8faa9896120}`

# _The Breakage_

Bài cho 2 tệp .img của 2 điện thoại android, load vô FTK để extract `data` rồi phân tích

![image](https://github.com/user-attachments/assets/e0019bfe-ba00-48dd-bd73-3e72330cd5c2)

Bài mô tả cần `PIN` của phone1 và `Password` của phone2 để tạo thành flag `DVCTF{PIN:PASSWORD}`, hướng đi là crack bằng hashcat

Bước đầu là phải dựng `hash` của cả 2 máy, đọc blog [này](https://www.pentestpartners.com/security-blog/cracking-android-passwords-a-how-to/) là làm được

- Phone 1:

![image](https://github.com/user-attachments/assets/25432728-b3fa-4802-ae74-a1e0b2a9ea40)

Do `salt` lấy ra là số âm nên có 1 số khác biệt, dùng script sau:

```python
salt = -5927124708069638479

# Chuyển số âm thành 8 byte (big endian, bù hai)
salt_bytes = salt.to_bytes(8, byteorder='big', signed=True)

# Chuyển bytes thành chuỗi hex lowercase
salt_hex = salt_bytes.hex()

print(salt_hex)
```

`md5 + salt`: FD55A53B6A21E46C41F82C2FDAE82620:adbe9f7b34117eb1

Tiến hành crack: `hashcat -m 10 FD55A53B6A21E46C41F82C2FDAE82620:adbe9f7b34117eb1 -a 3 ?d?d?d?d?d?d?d?d`

![image](https://github.com/user-attachments/assets/5453d837-08b5-4ff4-9565-43b1276bd147)

=> PIN: `24681379`

- Phone 2: Tương tự Phone 1

`md5 + salt`: 1F2E0A7B9912B973B9FB1BB6AADCDE7C:9fb610735c471ce4

`hashcat -m 10 1F2E0A7B9912B973B9FB1BB6AADCDE7C:9fb610735c471ce4 /home/kali/Documents/rockyou.txt `

![image](https://github.com/user-attachments/assets/202fb3ca-633e-4d41-8daf-8f16609bd292)

=> PASSWORD: `blueice309`

```FLAG: DVCTF{24681379:blueice309}```
