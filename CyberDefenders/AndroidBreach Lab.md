# _AndroidBreach Lab_

![image](https://github.com/user-attachments/assets/3ec7808e-5d0c-4d92-80c1-db282ccc5843)

## _Description_

At BrightWave Company, a data breach occurred due to an employee's lack of security awareness, compromising his credentials. The attacker used these credentials to gain unauthorized access to the system and exfiltrate sensitive data. During the investigation, the employee revealed two critical points: first, he stores all his credentials in the notes app on his phone, and second, he frequently downloads APK files from untrusted sources. Your task is to analyze the provided Android dump, identify the malware downloaded, and determine its exact functionality.

## _Solution_

> Q1. What suspicious link was used to download the malicious APK from your initial investigation?

Vì data được lấy từ điện thoại android nên mình dùng ALEAPP để phân tích

![image](https://github.com/user-attachments/assets/569a90a0-e506-4d1d-b0e4-729412998bcf)

Tìm suspicious link, mình vào phần _Chrome-Downloads_ thấy có đúng một đường link và cũng chính là đáp án

![image](https://github.com/user-attachments/assets/ef67dc46-6b7c-4a86-8108-985d5432aa3c)

```
Answer: https://ufile.io/57rdyncx
```

> Q2. What is the name of the downloaded APK?

Mình tìm các phần Downloads trong ALEAPP report nhưng không có, dùng hint thi được dẫn đến path __data/media/0/Download__

![image](https://github.com/user-attachments/assets/9a596f4f-cc10-47f9-a591-4cfc9ae382f1)

```
Answer: Discord_nitro_Mod.apk
```

> Q3. What is the malicious package name found in the APK?

Câu này mình phải dùng hint 1 thì được chỉ cho dùng tool JADX

Ngồi lướt 1 lượt khá lâu thì mình tìm thấy packet có tên __com.example.keylogger__ thấy có các hàm như mã hóa AES, sendmail thì mình đoán đây là malicious packet. Nhập vô thì thấy đúng

![image](https://github.com/user-attachments/assets/32f30241-3119-408d-985d-44d9a283f1ab)

```
Answer: com.example.keylogger
```

> Q4. Which port was used to exfiltrate the data?

Vào thẳng hàm SendEmail là thấy được

![image](https://github.com/user-attachments/assets/bfd1494f-1484-4ce7-85a3-72bfec8161b1)

```
Answer: 465
```

> Q5. What is the service platform name the attacker utilized to receive the data being exfiltrated?

Vẫn trong đoạn mã trên là ta thấy được host được sử dụng mailtrap.io

```
Answer: mailtrap.io
```

> Q6. What email was used by the attacker when exfiltrating data?

Trong SendEmail class không hiển thị trực tiếp về email của attacker, mà thông tin này được thiết lập trong __BroadcastforAlarm class__

![image](https://github.com/user-attachments/assets/ae835929-4ab9-407e-822c-831394a1e411)

```
Answer: APThreat@gmail.com
```

:)) Có 1 sự số là đang làm lại để viết wu thì lab bị khóa, méo hiểu

![image](https://github.com/user-attachments/assets/17fef572-c59b-4af7-82c7-3d7150477b56)

Đăng nhập bằng acc khác thì lại được, tiếp tục nào

![image](https://github.com/user-attachments/assets/b5f32285-1ac1-4a73-924c-6a1dd8c2a7fb)

> Q7. The attacker has saved a file containing leaked company credentials before attempting to exfiltrate it. Based on the data, can you retrieve the credentials found in the leak?

![image](https://github.com/user-attachments/assets/ae835929-4ab9-407e-822c-831394a1e411)

Cùng phân tích tiếp đoạn mã vừa nãy, có thể thấy chương trình trên thực hiện thu thập dữ liệu từ bàn phím người dùng, sau đó gửi cho attacker, cuối cùng là lưu chúng vào file config.txt. 

Tìm đến file đó là mình sẽ có được thông tin đăng nhập của người dùng

![image](https://github.com/user-attachments/assets/fc047302-2a0e-4d07-9133-20c072b5a174)

```
Answer: hany.tarek@brightwave.com:HTarek@9711$QTPO309
```

> Q8. The malware altered images stored on the Android phone by encrypting them. What is the encryption key used by the malware to encrypt these images?

Hỏi về mã hóa thì mình nhảy đến AES class nhưng không thấy, class này chỉ dùng để decode khóa được lưu dưới dạng base64, và tiến hành mã hóa, còn key thực sự được thiết lập trong MainTask, vào hàm Main là chúng ta sẽ lấy được key

![image](https://github.com/user-attachments/assets/9aa501b2-0974-42f5-a5ff-25d6df8439e0)

```
Answer: 9bY$wQ7!cTz465TX
```

> Q9 .The employee stored sensitive data in their phone's gallery, including credit card information. What is the CVC of the credit card stored?

Cái này mình phải dùng hint, tìm đến __data\media\0\Pictures\.aux__

![image](https://github.com/user-attachments/assets/9a2f4272-7637-426f-9231-ba34c4454881)

```
Answer: 128
```

