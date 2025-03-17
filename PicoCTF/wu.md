# _Ph4nt0m 1ntrud3r_ _(FORSENSICS)_
![image](https://github.com/user-attachments/assets/5a40462e-0cc1-4cd5-a308-867ef36526cc)

## _Solution_

Bài 50 points thì chỉ cần mở ra là có flag

![image](https://github.com/user-attachments/assets/8c9a2048-3d27-49d4-a18b-a201cc9606c7)

# _RED_ _(FORSENSICS)_

![image](https://github.com/user-attachments/assets/d16aaac2-9050-442d-bb8e-279ca975f455)

## _Solution

Bài này dùng _aperisolve_ là done

![image](https://github.com/user-attachments/assets/8cf182d8-5814-432a-8d4a-fadbaa48c0b1)

Decode base64

![image](https://github.com/user-attachments/assets/8b3dc6ee-4f92-4ff2-a55e-32a86d7c1a15)

# _Flag are stepic_ _(FORSENSICS)_

Bài cho 1 trang web gồm các lá cờ, theo gợi ý thì tìm lá cở của đất nước không tồn tại là sẽ có flag

![image](https://github.com/user-attachments/assets/18de0ec4-6fa5-4856-80eb-54618cfa2d6d)

Mình tìm đc cái này, save nó về

Dùng thư viện stepic của python là có flag

```python
import stepic
from PIL import Image

# Mở ảnh đã giấu tin
img = Image.open("upz.png")

# Giải mã dữ liệu
hidden_data = stepic.decode(img)
print("Dữ liệu ẩn:", hidden_data)
```

```
FLAG: picoCTF{fl4g_h45_fl4g51d83cb1}
```
# _Bitlocker-1_ _(FORSENSICS)_

![image](https://github.com/user-attachments/assets/c8c75875-491a-4382-8bd8-720dc64db09e)

Lần đầu làm việc với disk bị encrypt bằng bitlocker, tìm trên mạng 1 lúc thì ra được cái [này](https://github.com/GTekSD/BitLocker-CTF/blob/main/answer/you-looser/u-need-to/learn/a-lot/kiddo/go/see-this/answer.md)

Đầu tiên là trích xuất hash ra bằng bitlocker2john, lấy chuỗi _User Password hash_ lưu vào file riêng

![image](https://github.com/user-attachments/assets/5e31025c-55a4-45b4-ad23-98a48712f305)

Sau đó dùng hashcat để crack

![image](https://github.com/user-attachments/assets/9956605f-00e9-4f42-a8da-22a4213a3761)

Đem lên ArsenalImageMounter để mount và dùng pass là jacqueline

![image](https://github.com/user-attachments/assets/15b36173-b814-4f89-b6e4-c580d84758e1)

Vào ổ G mở ra là có flag trong flag.txt

```
FLAG: picoCTF{us3_b3tt3r_p4ssw0rd5_pl5!_3242adb1}
```

# _Event-Viewing_ _(FORSENSICS)_

![image](https://github.com/user-attachments/assets/2c0a731d-f5f2-4b5f-baa2-3703341d8b80)

## _Solution_

Này dựa theo mô tả để mình lọc event ID cho nhanh chứ ngồi đọc hết cũng ốm luôn

Đầu tiên là tải phần mềm, lọc ID 1033

![image](https://github.com/user-attachments/assets/eebd3748-1da3-45e2-a005-e87b545b2b62)

Tiếp theo, khi chạy phần mềm nhưng lại kh có gì, khả năng malware đã tự chỉnh sửa registry để tự động chạy khi khởi động ( ID: 4657 )

![image](https://github.com/user-attachments/assets/3563343b-d49c-4410-9d9b-fa25b07998a3)

Cuối cùng là khi đăng nhập vô máy thì bị shutdown luôn (ID: 1074)

![image](https://github.com/user-attachments/assets/cbe8e53c-68a9-4041-9122-823637b418b0)

Ghép các mảnh base64 ở phần detail lại là có được flag

![image](https://github.com/user-attachments/assets/8a3fb7fd-cf11-4596-b177-08268c1932db)

# _Bitlocker-2_ _(FORSENSICS)_

![image](https://github.com/user-attachments/assets/72b8a89b-25cb-4420-939d-5e8da306162f)

## _Solution_

Bài này cho 2 file, 1 file .dd bị bitlocker encrypt và 1 file dump bộ nhớ 

Để khôi phục disk đã bị mã hóa bằng bitlocker thì cơ bản có 3 cách:
- Password ( Làm như bài  __Bitlocker-1__ ) nhưng tỉ lệ rất thấp tại ông này đã đặt password mạnh hơn
- Recovery Key
- Fevk

Đầu tiên, mình chưa biết đến Fevk nên đã cố gắng tìm được Recovery Key trong file dump bằng cách dùng kha khá plugin của __volatility__ mình biết nhưng đều không thu thập được gì

Sau đó biết được còn 1 loại khóa nữa là __FEVK__

> Khi BitLocker mã hóa ổ đĩa, nó tạo ra một khóa chính gọi là Full Volume Encryption Key (FVEK) và nếu máy tính đang mở khóa ổ đĩa, FVEK có thể nằm trong RAM và có thể trích xuất bằng Volatility.

Theo mô tả, thì ram được dump ra khi ổ đĩa đang mở khóa nên mình sẽ chuyển hướng sang trích xuất __FEVK__

Các plugin mặc định của volatility đều không lấy được __FEVK__ nên phải tải về từ bên ngoài cụ thể là plugin __bitlocker__ tại [đây](https://github.com/breppo/Volatility-BitLocker?fbclid=IwY2xjawJAA29leHRuA2FlbQIxMAABHVs6ODOFU7pXKrSdGYMPOPWZFFyYyzJ4ctZewyaMs8l6rglCZ3tCWED-Fw_aem_1v8uIjbEQWDc6_6FA1mxNg)

Clone về rồi cp vô plugin của volatility thôi

Dùng vol2 nên cần xác định profile

![image](https://github.com/user-attachments/assets/de03d820-64df-4875-93a8-97bb85c02e6d)

Tiếp theo là trích xuất FEVK

![image](https://github.com/user-attachments/assets/ec29f7d2-869c-461f-98c0-d4c313df76d0)

Lấy luôn key ở đầu để decrypt ổ

![image](https://github.com/user-attachments/assets/48f01cb3-0314-45eb-8e2a-656751a4dcd2)

Mình dùng lệnh mount ngay trên kali nhưng bị lỗi, lười fix vl nên xem hex thì thấy có định dạng hệ thống tập tin là NTFS

![image](https://github.com/user-attachments/assets/626dfbb2-593b-4d6a-9720-4434b06dec02)

Đến đây mình nghĩ là có thể mount vô FTK IMAGER để đọc nội dung 

![image](https://github.com/user-attachments/assets/6dd9ea15-814a-405d-843b-21e1c23f084e)

Và đã lấy được flag ở file flag.txt nằm trong __/root__

![image](https://github.com/user-attachments/assets/bf6b42cf-c166-49ee-9f70-fe3738e191b9)

```
FLAG: picoCTF{B1tl0ck3r_dr1v3_d3crypt3d_9029ae5b}
```




