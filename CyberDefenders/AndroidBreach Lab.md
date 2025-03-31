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

Mình tìm các phần Downloads trong ALEAPP report nhưng không có, nên chuyển hướng tìm tại các thư mục có trong data trên máy
