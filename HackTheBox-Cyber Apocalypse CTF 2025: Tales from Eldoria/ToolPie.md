# _ToolPie_ _(FORSENSICS)_

![image](https://github.com/user-attachments/assets/40e9875f-1e6b-41c3-8c6d-30b24e0c8cc3)

Bài cho duy nhất 1 file pcapng. Hết, nhiệm vụ là phân tích để trả lời câu hỏi

>1.What is the IP address responsible for compromising the website?

Đầu tiên, cùng kiểm tra lưu lượng mạng

![image](https://github.com/user-attachments/assets/9e7760fa-4816-4245-ae06-0af56755f789)

Ta thấy luồng stream 4 với ip.src == 172.31.47.152 && ip.des == 13.61.177.218 khá đáng ngờ vì dữ liệu tải lên rất lớn (9MB), có thể là payload của tấn công

Lọc stream 4

![image](https://github.com/user-attachments/assets/c70aa52f-e0d6-4cc0-9515-e3654c8df1df)

Có thể thấy client thực hiện gửi một chuỗi __ec2amaz-bktvi3e\administrator__, đây có thể là tên máy và tài khoản Administrator. Cuối cùng client gửi 1 chuỗi dữ liệu giống như 1 file dữ liệu nào đó

=> Có thể ấn công tải xuống dữ liệu nhạy cảm từ server mục tiêu

```
Answer: 13.61.177.218
```

>2. What is the name of the endpoint exploited by the attacker?

