# _TRUE SECRETS_
![image](https://github.com/4venger/CTF-writeups/blob/main/HTB/Screenshot%202025-01-15%20031111.png)
Chal này được tag easy nhưng với trình độ của mình hiện tại thì mình thấy khó vl
## _Solution_
Tải tệp zip về giải nén thì được 1 file .raw

![image](https://github.com/user-attachments/assets/549c1ef3-aff8-4cf4-94b0-8e01868e9213)

Theo mô tả của bài _"...Fortunately, our unit was able to raid the home of the leader of the APT group and take a memory capture of his computer while it was still powered on..."_ thì mình đoán thử thách này liên quan đến **MEMORY DUMP**

Mình dùng **volatility3** với plugin là windows.pstree để xem các tiến trình

![image](https://github.com/user-attachments/assets/f9b50a22-9aef-4766-8353-6ee47772a52e)

Xem qua một lượt thì mình thấy toàn những tiến trình quen thuộc của windows như **svchost.exe**, **expoler.exe**, ... và chúng đều không nằm ở các đường dẫn lạ

Lướt xuống cuối thì thấy 2 tiến trình khá lạ là *True Crypt.exe* và *7zFM.exe*

![image](https://github.com/user-attachments/assets/7baf7c5b-fdb8-4298-b803-963b1a9271ab)

Nên mình thử dùng plungin **windows.dumpfiles** để trích xuất hết các file có trong đó với PID của thằng ***True Crypt.exe*** là 2128

![image](https://github.com/user-attachments/assets/9c9df0ff-261f-455d-a0ee-4fc64eacf6bc)









