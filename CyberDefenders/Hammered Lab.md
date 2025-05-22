# _Hammered Lab_

![image](https://github.com/user-attachments/assets/cbede024-13f6-4a4e-8ee0-c8ca279d8440)

## _Solution_

![image](https://github.com/user-attachments/assets/bdede07a-9cf8-40d6-8cf1-a4aaa0497533)

| Tên File            | Mục Đích / Nội Dung Chính                                                                 | Ví Dụ Thông Tin Có Thể Tìm Được                                           |
|---------------------|--------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| `auth.log`          | Ghi lại hoạt động xác thực: SSH, sudo, brute-force, đăng nhập                             | SSH từ IP lạ, dùng sudo, lỗi xác thực                                    |
| `daemon.log`        | Log của các dịch vụ chạy nền (cron, bluetooth, rsyslog...)                                 | Cron job lạ, tiến trình nền bất thường                                   |
| `debug`             | Log dùng để gỡ lỗi hệ thống, các thông tin chi tiết từ service hoặc kernel                 | Lỗi kết nối mạng, lỗi phần mềm                                            |
| `dmesg`, `dmesg.0`  | Log khởi động hệ thống, thông tin từ kernel, phần cứng                                      | Cắm USB, khởi động card mạng, lỗi driver                                 |
| `dpkg.log`          | Ghi lại lịch sử cài đặt/gỡ gói thông qua `dpkg` hoặc `apt`                                 | Cài tool đáng ngờ, gỡ bỏ phần mềm hệ thống                               |
| `fontconfig.log`    | Log liên quan đến font hệ thống, thường ít quan trọng                                      | Cảnh báo cấu hình font không đúng                                        |
| `fsck`              | Log kiểm tra file system khi có lỗi đĩa hoặc tắt máy đột ngột                              | Thông báo phân vùng sạch hoặc có lỗi                                     |
| `kern.log`          | Log của kernel: lỗi thiết bị, nạp module, cảnh báo                                         | Gắn thiết bị lạ, nạp module nguy hiểm                                    |
| `messages`          | Tổng hợp nhiều loại log hệ thống (tương tự syslog)                                         | Bắt đầu dịch vụ, cảnh báo phần cứng                                      |
| `secure`            | Tương tự `auth.log`, dùng trên CentOS/RHEL                                                 | Đăng nhập SSH, dùng sudo                                                 |
| `udev`              | Ghi lại thông tin phần cứng khi được gắn/gỡ (USB, ổ cứng, webcam...)                       | Phát hiện USB lạ được kết nối                                            |
| `user.log`          | Hoạt động từ các ứng dụng người dùng                                                       | Mở ứng dụng GUI, hoạt động keyring                                       |
| `apache2/access.log`| Ghi lại truy cập HTTP tới máy chủ Apache                                                   | IP nào truy cập trang web nào, phương thức GET/POST                      |
| `apache2/error.log` | Ghi lại lỗi dịch vụ web Apache                                                             | Cảnh báo lỗi PHP, lỗi quyền truy cập                                     |
| `apt/history.log`   | Log của `apt` khi cập nhật, nâng cấp, gỡ cài gói                                           | Thời điểm cập nhật hệ thống hoặc thêm tool                              |
