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

Dựa theo báo cáo của `virustotal` 

![image](https://github.com/user-attachments/assets/4bcd9924-3fc1-49a1-88e5-5e7bebb34300)

Kiểm tra bằng `netscan` thấy có `iexplore.exe` đang ở trạng thái `Established`

![image](https://github.com/user-attachments/assets/6b025422-0a71-4a24-a4b4-664f55425ae9)

Kết hợp với ip `180.76.254.120:22` cũng được tìm thấy trong trong report của `virustotal` 

![image](https://github.com/user-attachments/assets/79531105-657d-4bb1-b9df-c0d9d4eaea2d)

`Answer: 2996`

>Q5. Machine:Target1 What is the unique value the malware is using to maintain persistence after reboot?

Check các chỗ có thể thực hiện `persistence`, mình check trong `SOFTWARE\MICROSOFT\WINDOWS\CURRENTVERSION\RUN` bằng plugin  `printkey`

![image](https://github.com/user-attachments/assets/0ce47333-4343-468c-9818-402d78af6bc8)

`Answer: MrRobot`

>Q6. Machine:Target1 Malware often uses a unique value or name to ensure that only one copy runs on the system. What is the unique name the malware is using?

Câu hỏi này liên quan đến 1 kĩ thuật mà malware thường sử dụng là ngăn chặn 1 lúc có nhiều bản sao để tránh xảy ra xung đột khi thực thi. Dùng plugin `mutantscan`

`Mutex (mutant) là một loại đối tượng đồng bộ (synchronization object) trong Windows, dùng để ngăn nhiều tiến trình truy cập vào tài nguyên cùng lúc.`

![image](https://github.com/user-attachments/assets/301dfe78-7207-45f2-a862-322a713bb3cb)

Hoặc có thể xem trên `virustotal`

![image](https://github.com/user-attachments/assets/5e714fea-4cce-4a02-8973-ae70ae2b5594)

`Answer: fsociety0.dat`

>Q7. Machine:Target1 It appears that a notorious hacker compromised this box before our current attackers. Name the movie he or she is from.

![image](https://github.com/user-attachments/assets/de0e4aac-1370-4300-a69c-92abd85ec81c)

Dùng filescan grep `users` thì có được tên của một diễn viên nổi tiếng trong phim `Hackers`

![image](https://github.com/user-attachments/assets/bfc21757-55fa-439e-b952-61fc4ae0ced5)

![image](https://github.com/user-attachments/assets/326af1e6-bb32-4db8-b4f6-5d3fd48cb1bd)

`Answer: Hackers`

>Q8. Machine:Target1 What is the NTLM password hash for the administrator account?

```
┌──(kali㉿kali)-[~/Downloads/temp_extract_dir (3)/target1]
└─$ python2 /home/kali/volatility/vol.py -f Target1-1dd8701f.vmss --profile=Win7SP1x86_23418 hashdump                  
Volatility Foundation Volatility Framework 2.6.1
Administrator:500:aad3b435b51404eeaad3b435b51404ee:79402b7671c317877b8b954b3311fa82:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
front-desk:1000:aad3b435b51404eeaad3b435b51404ee:2ae4c526659523d58350e4d70107fc11:::
```

`Answer: 79402b7671c317877b8b954b3311fa82`

>Q9.Machine:Target1 The attackers appear to have moved over some tools to the compromised front desk host. How many tools did the attacker move?

![image](https://github.com/user-attachments/assets/8a38df59-af9b-4260-a85e-9b79500f8a24)

Hint cho mình hướng tìm trong `temp`

![image](https://github.com/user-attachments/assets/fdd8d860-12d0-4d93-81ed-53188312fdf0)

Có 3 thằng tool đáng ngờ là `wce.exe`, `getlsasrvaddr.exe` và `nbtscan.exe`

`Answer: 3`

>Q10. Machine:Target1 What is the password for the front desk local administrator account?

Dùng `hashdump` xác định được hash nhưng khả năng đặt pass mạnh nên kh crack được

Chuyển hướng sang kiểm tra `console` vì trong các tool mà attacker dùng có `wce.exe` đây là tool dùng để trích xuất password 

![image](https://github.com/user-attachments/assets/875c21d1-09ce-4f94-bde2-55d91c74c3cf)

Có thể thấy được attacker dùng `wce.exe -w` là hiển thị mật khẩu dưới dạng plaintext

`Answer: flagadmin@1234`

>Q11. Machine:Target1 What is the std create data timestamp for the nbtscan.exe tool?

`python2 /home/kali/volatility/vol.py -f Target1-1dd8701f.vmss --profile=Win7SP1x86_23418 timeliner > output1 `

![image](https://github.com/user-attachments/assets/75c6d7d6-1c93-41cb-966c-3565efa2b9e0)

`Answer: 2015-10-09 10:45:12 UTC`

>Q12. Machine:Target1 The attackers appear to have stored the output from the nbtscan.exe tool in a text file on a disk called nbs.txt. What is the IP address of the first machine in that file?

![image](https://github.com/user-attachments/assets/420ccca4-d050-4afa-a5f2-d2c7499e7df9)

![image](https://github.com/user-attachments/assets/7ce7fb80-0606-48fd-85d4-7e691bc31713)

`Answer: 10.1.1.2`

>Q13. Machine:Target1 What is the full IP address and the port was the attacker's malware using?

Ở câu 4 đã phân tích

`Answer: 180.76.254.120`

>Q14. Machine:Target1 It appears the attacker also installed legit remote administration software. What is the name of the running process?

![image](https://github.com/user-attachments/assets/a74d6941-120c-4a9d-8b7f-1862d5f51df3)

`Answer: TeamViewer.exe`

>Q15. Machine:Target1 It appears the attackers also used a built-in remote access method. What IP address did they connect to?

![image](https://github.com/user-attachments/assets/689c5262-990a-4cfc-934a-1346b8ca5ceb)

Thấy được attacker sử dụng cả RDP mặc định trong `windows`

`Answer: 10.1.1.21`

>Q16. Machine:Target2 It appears the attacker moved latterly from the front desk machine to the security admins (Gideon) machine and dumped the passwords. What is Gideon's password?

![image](https://github.com/user-attachments/assets/31d939d6-10db-4cd5-90b6-aa11c12c5cfa)

Thấy được attacker dùng `wce.exe` để lấy thông tin mật khẩu và lưu chúng tại `gideon/w.tmp`

![image](https://github.com/user-attachments/assets/fe180a10-4ea8-4dec-8ecb-015defdb303d)

![image](https://github.com/user-attachments/assets/297c7837-919e-49e9-9558-dcdb0f8c624d)

`Answer: t76fRJhS`

>Q17. Machine:Target2 Once the attacker gained access to "Gideon," they pivoted to the AllSafeCyberSec domain controller to steal files. It appears they were successful. What password did they use?

![image](https://github.com/user-attachments/assets/8ee4d558-27bd-43b1-be2a-88bdcd0ea940)

`Answer: 123qwe!@#`

>Q18. Machine:Target2 What was the name of the RAR file created by the attackers?

Tại câu 17 có luôn đáp án

`Answer: crownjewlez.rar`

>Q19. Machine:Target2 How many files did the attacker add to the RAR archive?

![image](https://github.com/user-attachments/assets/b0679147-37c1-4a30-bf44-52bc4ae9259b)

Attacker đã thực hiện kết nối tới 1 ổ đĩa ngoài (không phải cục bộ) nên không thể dùng `filescan` để quét 

Chuyển hướng sang việc dumpfile từ tiến trình thực thi

![image](https://github.com/user-attachments/assets/e2e9868b-8cb0-415d-9d0b-cdeac28fb641)

Dùng `memdump` thay vì `dumpfiles` vì cần dump toàn bộ bộ nhớ của tiến trình đó, chứ không chỉ riêng 1 số file riêng lẻ

`python2 /home/kali/volatility/vol.py -f target2-6186fe9f.vmss --profile=Win7SP1x86_23418 memdump --pid=3048 -D out`

Dumpfile thành công nhưng không đọc được luôn, tại windows sẽ mã hóa văn bản dưới dạng `UTF-16`, dùng `strings -el` để trích xuất và có thể đọc được những dữ kiện được mã hóa bằng `UTF-16`

![image](https://github.com/user-attachments/assets/f4e98509-e7c4-46b7-a87a-84ae7dc85810)

`Answer: 3`

>Q20. Machine:Target2 The attacker appears to have created a scheduled task on Gideon's machine. What is the name of the file associated with the scheduled task?

Tác vụ được lập lịch sẽ được lưu tại  `Windows\System32\Tasks`

![image](https://github.com/user-attachments/assets/f1f6d482-1518-4e36-bb75-319c33ee33b9)

Ngoài mấy thằng mặc định của `windows` thì lòi ra 1 anh `at1` khá khả nghi, dump nó về

![image](https://github.com/user-attachments/assets/36818458-e79d-47d2-bd2a-1944e299c182)

`Answer: 1.bat`

>Q21. Machine:POS What is the malware CNC's server?

Dùng `malfind` thấy cái này khả nghi nhất 

![image](https://github.com/user-attachments/assets/28e4327e-15b4-444b-bf2b-d8e6cea8509d)

Dùng `netscan` rồi grep nó

![image](https://github.com/user-attachments/assets/adf5cfa8-25a7-4ee8-b775-be4807a9fae4)

`Answer: 54.84.237.92`

>Q22. Machine:POS What is the common name of the malware used to infect the POS system?

![image](https://github.com/user-attachments/assets/a76e4bd1-4466-490d-8502-00cdea28ffbd)

![image](https://github.com/user-attachments/assets/eca12210-c0dc-4ade-a0ba-c05405628d7a)

`Answer: dexter`

>Q23. Machine:POS In the POS malware whitelist. What application was specific to Allsafecybersec?

