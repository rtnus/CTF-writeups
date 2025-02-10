# _Fishy HTTP (Forensics)_

![image](https://github.com/user-attachments/assets/6bbab75c-c332-4f82-9dcd-24380dfbeb52)

## _Solution_

Bài cho 1 file _sustraffic.pcapng_ và 1 file _smphost.exe_, mình dùng Wireshark để phân tích

![image](https://github.com/user-attachments/assets/6dbc3b4d-1966-4076-9417-746eee790870)

Có 104 packets và chỉ bao gồm 2 giao thức quen thuộc là TCP và HTTP. Như thường lệ mình sẽ export HTTP để xem có những gì

![image](https://github.com/user-attachments/assets/8ecfe5a2-fc5d-4882-9dcd-4f4cb5b12c67)

Mình save all, và mở ra đọc thì cũng méo hiểu lắm :)) Nên mình sẽ duyệt qua từng stream để xem nó làm gì

Bắt đầu với eq 0, chỉ là một yêu cầu client và phản hồi của server trả về kết quả hiển thị cho 1 trang web 

![image](https://github.com/user-attachments/assets/e04678c4-18e0-489b-8a27-39359d185340)

Đến với eq 1, thì cũng không có gì khi chỉ là client gửi một request đến đường dẫn _/submit_feedback_ và response của server là nội dung y hệt như đã gửi

![image](https://github.com/user-attachments/assets/b0112ce0-40e7-4901-8a0c-896f1031b271)

Mình xem qua tất cả thì cũng tương tự như 2 luồng vừa rồi, và mình cũng không hiểu gì :)) Nên mình chuyển hướng sang đi phân tích file .exe còn lại

Muốn decomplie thì phải xem nó đc viết bằng gì và sử dụng thử viện gì 

![image](https://github.com/user-attachments/assets/0abdd55e-67b5-4c9c-a154-4b3fa28026cc)

Có thể thấy chương trình sử dụng thư viện .NET nên có thể dùng ILSpy hoặc Dotpeek để decomplie, ở đây mình dùng ILspy

Tìm đến hàm main để phân tích 

![image](https://github.com/user-attachments/assets/b5386776-5b55-452b-992f-7b6bf8b31a9c)

Đọc qua thì thấy được viết bằng C# để thực hiện giao tiếp với server thông qua HTTP

Vừa xem thì đã thấy có cái gì đấy liên quan đến base64

![image](https://github.com/user-attachments/assets/8667538c-9ea5-4c9e-bb07-0669b8f7d0dc)

Đem đi decode cho chắc 

![image](https://github.com/user-attachments/assets/5c87764a-ba4f-4e0a-b2c4-844e81ecd5ed)

Hmm thấy được có một list từ tiếng anh được sắp xếp từ a->z 

Ok giờ sẽ đi lần lượt từng hàm xem nó làm gì

![image](https://github.com/user-attachments/assets/99d496dd-cf84-40c6-804f-00cb931284bc)

Hàm trên thực hiện công việc 

- Giải mã chuỗi base64 trên thành dạng byte[] -> sẽ là list các từ tiếng anh như trên đã decode
- Sau đó được chuyển thành chuỗi UTF-8 và loại bỏ các khoảng trắng không cần thiết.

Tiếp theo tạo một danh sách dictionary với các từ tiếng anh trên
- Sau đó duyệt qua từng từ trong danh sách
- Sử dụng ký tự đầu tiên của mỗi từ làm khóa trong dictionary
- Nếu khóa tồn tại thì thêm phần còn lại vào danh sách
- Không tồn tại tạo một ds mới chứa phần còn lại của từ

![image](https://github.com/user-attachments/assets/9d048377-f945-47ad-827a-2786cbf40169)

Ví dụ danh sách từ gồm

> apple apple airplane angle banana ball

thì wordsDict sẽ được tạo như sau: 

```
{
    'a' -> ["pple", "irplane", "ngle"],
    'b' -> ["anana", "all"]
}
```




