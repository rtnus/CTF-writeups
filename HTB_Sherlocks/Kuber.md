![Screenshot 2025-05-03 163623](https://github.com/user-attachments/assets/731b401a-a99d-4b6d-9acd-08a98060b4fb)

| Tài nguyên        | Mô tả                                                                                   | Mục đích sử dụng                                                      |
|-------------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| **Pods**          | Đơn vị nhỏ nhất chứa container trong Kubernetes. Mỗi Pod có thể chứa một hoặc nhiều container. | Dùng để test hoặc chạy các container đơn lẻ.                          |
| **ConfigMaps**    | Lưu trữ cấu hình không nhạy cảm như text, biến môi trường, file cấu hình...              | Tách cấu hình khỏi mã nguồn. Container lấy cấu hình qua biến môi trường hoặc mount vào file. |
| **Secrets**       | Lưu trữ dữ liệu nhạy cảm như mật khẩu, token, SSH key... Mã hóa bằng Base64.              | Cung cấp dữ liệu bí mật cho Pod hoặc Deployment.                       |
| **DaemonSets**    | Triển khai một Pod trên mỗi Node trong cluster.                                          | Thường dùng cho các tiến trình nền như log collector, monitoring agent. |
| **Jobs**          | Chạy tác vụ ngắn hạn rồi kết thúc. Sau khi hoàn thành, Pod tự động kết thúc.             | Dùng cho các công việc như sao lưu dữ liệu, xử lý file, gửi email hàng loạt. |
| **ServiceAccounts**| Tạo tài khoản dịch vụ cho Pod để truy cập API hoặc tài nguyên khác.                     | Dùng khi Pod cần quyền truy cập đặc biệt, kết hợp với RBAC để phân quyền. |
| **Namespaces**    | Tạo namespace để nhóm các tài nguyên Kubernetes lại với nhau.                           | Dùng để chia môi trường như development, testing, production. Quản lý tài nguyên trong cluster. |
