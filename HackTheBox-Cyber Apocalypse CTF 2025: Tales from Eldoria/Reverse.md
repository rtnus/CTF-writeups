# _SealedRune_ _(REVERSE)_

![image](https://github.com/user-attachments/assets/49ee50de-05e1-466a-88b6-d971059ead4e)

Bài cho file elf, mình dùng ida phân tích 

![image](https://github.com/user-attachments/assets/39513ae9-d386-4e6c-bac2-f27c646d9b6f)

Flag bị mã hó base64 rồi reverse

![image](https://github.com/user-attachments/assets/7cc1ccb4-af90-4c77-bbdb-6d8fcfe25141)

```
HTB{run3_m4g1c_r3v34l3d}
```

# _EncryptedScroll_ _(REVERSE)_

![image](https://github.com/user-attachments/assets/dffd1e23-7128-4a6f-9284-44f26444f373)

Khá nhiều hàm nhưng chỉ cần chú ý vào hàm decrypt_message là có được flag

![image](https://github.com/user-attachments/assets/808f4640-26e1-43d7-8103-bb156c533534)

Đầu vào là chuỗi a1, khởi tạo s2, sau đó dùng for duyệt qua mảng s2, giảm mỗi từ trong s2 đi 1 đơn vị sau đó so sánh a1 = s2 thì in "The Dragon's Heart...", khác thì "The scroll...".

```python
def decrypt_message(input_str):
    # Chuỗi mã hóa ban đầu từ mã C
    s2 = "IUC|t2nqm4`gm5h`5s2uin4u2d~"
    
s2 = "IUC|t2nqm4`gm5h`5s2uin4u2d~"
decoded_s2 = ''.join(chr(ord(c) - 1) for c in s2)
print(f"Chuỗi giải mã từ s2: {decoded_s2}")
```

```
HTB{s1mpl3_fl4g_4r1thm3t1c}
```
