# Phantom Connection
Like a fleeting dream, a connection once existed but has faded into the void. Only shadows of its presence remain. Can you bring it back to light?

# _Solution_

Bài cho file .zip, giải nén được 2 file 

![image](https://github.com/user-attachments/assets/931621d0-4b40-44e0-b770-861c01370a76)

Search mạng lúc ra tool [này](https://github.com/ANSSI-FR/bmc-tools), làm theo hướng dẫn thôi

![image](https://github.com/user-attachments/assets/455d0a11-7a1b-4c8b-98a5-beb354badf3d)

Có rất nhiều ảnh bị cắt nhỏ lướt 1 lúc là thấy flag

![image](https://github.com/user-attachments/assets/5686c27e-1b1a-4c56-a326-9f142f3c8827)

```
FLAG: apoorvctf{CAcH3_Wh4t_YoU_sE3}
```
# Samurai’s Code

Unveil the lost code of the Samurai and unlock the mystery hidden within.

# _Solution_

Bài cho 1 file ảnh bth kh có j đặc biệt, thử xem hex file

![image](https://github.com/user-attachments/assets/54ca7f83-e681-4073-9218-e96d66776d2b)

Dùng [đây](https://www.dcode.fr/brainfuck-language) để decode ra 1 link drive

![image](https://github.com/user-attachments/assets/641a67d0-1291-41d3-a86c-6f87287e3355)

Tải về nhận đc 1 file không có extension, đem vô HxD

![image](https://github.com/user-attachments/assets/e6acc27c-87b8-498a-b37e-f82830399a8a)

Có thể thấy khá giống với file jpg nhưng có gì đó lạ lạ, nhận ra 2 byte liền kề nhau phải đổi chỗ nhau thì mới ra được file chuẩn

Script:

```python
with open("samurai", "rb") as f:
    data = f.read()

# Đổi chỗ từng cặp byte
fixed_data = b"".join(data[i:i+2][::-1] for i in range(0, len(data), 2))

with open("fixed2.jpg", "wb") as f:
    f.write(fixed_data)
```
![image](https://github.com/user-attachments/assets/79ae41c0-6867-49d2-98a3-3a648c79a509)

```
FLAG: apoorvctf{ByT3s_OUT_OF_ORd3R}
```

# Ramen lockdown

A criminal mastermind named larry stole Chef Tataka ultimate ramen recipe and yeeted it into a password-protected zip file. Inside? A sacred file with the secret to flavor nirvana. Crack the zip, save the slurp. No pressure. 🍜💀

# _Solution_

Bài bắt crack file zip, dùng rất nhiều tool nhưng kh đc thì cuối cùng cũng tìm đc cái này [đây](https://github.com/kimci86/bkcrack)

Tool yêu cầu phải biết tối thiểu 12 bytes về dữ liệu bên trong, nên biết bên trong có file PNG nên lưu phần chunk mà mọi tệp PNG nào cũng có là PNG và IHDR

![image](https://github.com/user-attachments/assets/da6764ce-7e62-48ab-b8ad-90cc68230cd8)

Giờ theo hướng dẫn thôi, nhưng biết có bằng đấy byte thì bẻ khóa khá lâu ( khoảng 10' ) 

![image](https://github.com/user-attachments/assets/3c29b55e-db0a-4c54-a3c6-0cc61d6a664e)

Dùng key này để extract mà không cần pass

![image](https://github.com/user-attachments/assets/86bd348a-142d-415f-8ca1-9e491b3e78e2)

Done

![image](https://github.com/user-attachments/assets/3f3663ca-b2f3-4b33-a05a-3ad5d2857953)

```
FLAG: apoorvctf{w0rst_r4m3n_3v3r_ong}
```
