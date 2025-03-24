# _Stealth Invasion_ _(FORSENSICS)_

![image](https://github.com/user-attachments/assets/cf31068e-2c57-45e6-abe8-f1d8833dbef8)

Bài cho file memdump.elf, lần đầu mình gặp file dump mà đuôi là elf nên liền check file cái đã

![image](https://github.com/user-attachments/assets/778ce1f0-fd87-4a22-9e00-35ad1ca31f8c)

Vẫn đúng là file dump bth thôi, giờ dùng volatility để phân tích

>1. What is the PID of the Original (First) Google Chrome process:

Câu này dùng plugin windows.pslist

![image](https://github.com/user-attachments/assets/6a2f80f8-2693-4f1e-9c46-8efba4fc9972)

```
Answer: 4080
```

>2. What is the only Folder on the Desktop

Câu này dùng windows.filescan | grep desktop

![image](https://github.com/user-attachments/assets/d81d15a8-c9cf-43ae-8d13-454f6fdb9c5e)

```
Answer: malext
```

> 3. What is the Extention's ID (ex: hlkenndednhfkekhgcdicdfddnkalmdm)

Tiếp tục dùng filescan, mình grep extension nhưng không ID nào đúng nên mình sẽ grep chrome ( vì có liên quan đến câu hỏi 1 )

![image](https://github.com/user-attachments/assets/b6e5be76-03b5-417f-8f83-924eb40ad819)

```
Answer: nnjofihdjilebhiiemfmdlpbdkbjcpae
```

>4. After examining the malicious extention's code, what is the log filename in which the datais stored

Tiếp tục là filescan và grep ID vừa rồi tìm được

![image](https://github.com/user-attachments/assets/768e0890-c371-4dbf-bc88-209feeff45c2)

```
Answer: 000003.log
```
>5. What is the URL the user navigated to

Mình dumpfiles 000003.log này về

![image](https://github.com/user-attachments/assets/5b04628a-41ac-486f-8a82-4546851c4502)

Tệp log này ghi lại lịch sử nhấn phím của người dùng

![image](https://github.com/user-attachments/assets/499934aa-215c-46f2-9eff-6e64b314a0ca)

Có thể thấy người dùng truy cập vào drive.google.com rồi thực hiện login, nên từ đây có thể lấy được luôn đáp án cho câu 6

```
Answer: drive.google.com
```

>6. What is the password of selene@rangers.eldoria.com

![image](https://github.com/user-attachments/assets/64d9d6ae-fefa-49ab-a12b-79b7dc8f1d75)

```
Answer: clip-mummify-proofs
```
