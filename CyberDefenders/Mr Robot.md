# Mr Robot

![image](https://github.com/user-attachments/assets/67e04b4d-6332-4982-8afa-bbbf75f041be)

## Solution

>Q1. Machine:Target1 What email address tricked the front desk employee into installing a security update?

Dùng pslist để xem các tiến trình, thấy có `Outlook.exe` lấy pid của nó về

![image](https://github.com/user-attachments/assets/f95c9e31-09eb-4d8b-a539-48bee074b464)

Dùng `Dùng yarascan` để thực hiện quét trong ram dump, thực ra có thể dùng strings nhưng dùng cái này sẽ được thu hẹp phạm vi chỉ của process  `proccess`

`python2 /home/kali/volatility/vol.py -f Target1-1dd8701f.vmss --profile=Win7SP1x86_23418 yarascan --yara-rules="From:" --wide -p 3196`

Giải thích 1 chút:

| Tham số                     | Ý nghĩa                                                                 |
|-----------------------------|------------------------------------------------------------------------|
| `--yara-rules="From:"`      | Dùng chuỗi `"From:"` làm **quy tắc YARA** đơn giản, tìm chuỗi trong vùng nhớ |
| `--wide`                    | Quét cả chuỗi **Unicode (UTF-16LE)** – rất cần khi tìm chuỗi trong hệ điều hành Windows |
| `-p 3196`                   | **Chỉ quét vùng nhớ** thuộc về tiến trình có **PID = 3196**, giúp thu hẹp phạm vi tìm kiếm |

![image](https://github.com/user-attachments/assets/e48d4821-b970-4aec-847b-86f9b7b88dc0)

`Answer: th3wh1t3r0s3@gmail.com `

>Q2. Machine:Target1 What is the filename that was delivered in the email?

Vẫn dùng `yarascan` nhưng giờ sẽ from `.exe` vì liên quan đến file 

![image](https://github.com/user-attachments/assets/2bf3c7da-322e-48ef-9027-7dc797225e9c)


`Answer: AnyConnectInstaller.exe`

>Q3. Machine:Target1 What is the name of the rat's family used by the attacker?

Dùng `filescan` grep file vừa tìm được rồi thực hiện dump ra để lấy hash đem lên `virustotal`

```
┌──(kali㉿kali)-[~/Downloads/temp_extract_dir (3)/target1]
└─$ python2 /home/kali/volatility/vol.py -f Target1-1dd8701f.vmss --profile=Win7SP1x86_23418 filescan | grep -i "AnyConnectInstaller.exe"
Volatility Foundation Volatility Framework 2.6.1
0x000000003debbf80      8      0 R--r-- \Device\HarddiskVolume2\Windows\Prefetch\ANYCONNECTINSTALLER.EXE-BF8040D4.pf
0x000000003df12dd0      2      0 RW-rwd \Device\HarddiskVolume2\Users\anyconnect\AnyConnect\AnyConnectInstaller.exe
0x000000003df1cf00      4      0 R--r-d \Device\HarddiskVolume2\Users\anyconnect\AnyConnect\AnyConnectInstaller.exe
0x000000003e0bc5e0      7      0 R--r-d \Device\HarddiskVolume2\Users\frontdesk\Downloads\AnyConnectInstaller.exe
0x000000003e2559b0      8      0 R--rwd \Device\HarddiskVolume2\Users\frontdesk\Downloads\AnyConnectInstaller.exe
0x000000003e2ae8e0      8      0 RWD--- \Device\HarddiskVolume2\Users\anyconnect\AnyConnect\AnyConnectInstaller.exe
0x000000003ed57968      4      0 R--r-d \Device\HarddiskVolume2\Users\frontdesk\Downloads\AnyConnectInstaller.exe
0x000000003fc3c8c0      8      0 R--r-- \Device\HarddiskVolume2\Windows\Prefetch\ANYCONNECTINSTALLER.EXE-F5AF5299.pf
^C
                                                                                                                                                 
┌──(kali㉿kali)-[~/Downloads/temp_extract_dir (3)/target1]
└─$ python2 /home/kali/volatility/vol.py -f Target1-1dd8701f.vmss --profile=Win7SP1x86_23418 dumpfiles -Q 0x3e2559b0 --dump-dir=out
Volatility Foundation Volatility Framework 2.6.1
DataSectionObject 0x3e2559b0   None   \Device\HarddiskVolume2\Users\frontdesk\Downloads\AnyConnectInstaller.exe
                                                                                                                                                 
┌──(kali㉿kali)-[~/Downloads/temp_extract_dir (3)/target1]
└─$ md5sum out/file.None.0x85afe708.dat      
b05e889e8436ed89157a2aa0cb8cdaa6  out/file.None.0x85afe708.dat
                                                                                                                                                 
┌──(kali㉿kali)-[~/Downloads/temp_extract_dir (3)/target1]
└─$ python2 /home/kali/volatility/vol.py -f Target1-1dd8701f.vmss --profile=Win7SP1x86_23418 dumpfiles -Q 0x3e0bc5e0 --dump-dir=out
Volatility Foundation Volatility Framework 2.6.1
ImageSectionObject 0x3e0bc5e0   None   \Device\HarddiskVolume2\Users\frontdesk\Downloads\AnyConnectInstaller.exe
DataSectionObject 0x3e0bc5e0   None   \Device\HarddiskVolume2\Users\frontdesk\Downloads\AnyConnectInstaller.exe
                                                                                                                                                 
┌──(kali㉿kali)-[~/Downloads/temp_extract_dir (3)/target1]
└─$ md5sum out/file.None.0x85cd09a0.img 
165a952830dbd91509c48a3275edc379  out/file.None.0x85cd09a0.img
```

Có khá nhiều file nhưng chỉ có 1 tại address `0x000000003e0bc5e0` là check đỏ ngầu (cũng méo hiểu)

![image](https://github.com/user-attachments/assets/dc69401c-4216-4448-afcf-50a37eb8534b)

`Answer: XTREMERAT`

>Q4. Machine:Target1 The malware appears to be leveraging process injection. What is the PID of the process that is injected?

### too lazy to write hmm
