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

![image](https://github.com/user-attachments/assets/9387447b-5616-40fd-8263-1b9bb131fe3b)

Hàm này trích xuất nội dung nằm trong _<body>...</body>_ sau đó chuỗi bên trong được tách thành từng dòng
- Duyệt qua từng thẻ, nếu không phải  thẻ _li_ thì lấy tên thẻ và tìm trong tagHex
- Giá trị tương ứng của thẻ trong tagHex được nối thành chuỗi và chuyển đổi thành dạng byte[]

![image](https://github.com/user-attachments/assets/85283dfd-5963-4f21-b4f3-f37e00167001)

Hàm EncodeData sẽ chuyển data thành chuỗi Base64, duyệt từng kí tự trong chuỗi
- Nếu kí tự có trong wordsDict, thêm một chuỗi ngẫu nhiên từ wordsDict vào sau nó
- Nếu không thì chỉ thêm kí tự vào kết quả

Hàm HexStringToBytes chuyển mã hex thành mảng byte, giải mã mảng byte sang chuỗi ASCII và trả về chuỗi kết quả

![image](https://github.com/user-attachments/assets/ef5fdea0-82c1-4a8f-a38a-07bdae45e7f3)

Hàm SendRequest() trong đoạn code C# thực hiện các thao tác sau:
- Tạo một HTTP client để gửi yêu cầu GET đến một URL (url).
- Nhận dữ liệu phản hồi từ server, sau đó giải mã (DecodeData()) nội dung phản hồi.
- Chạy một tiến trình mới (cmd.exe) với tham số là kết quả đã giải mã.
- Đọc kết quả đầu ra của lệnh đã chạy trong cmd.exe.
- Kiểm tra kết quả, nếu đầu ra rỗng, gán giá trị "succeed".
- Mã hóa lại dữ liệu (EncodedData()) và gửi nó lên server thông qua HTTP POST request.

Sau những gì phân tích, thì có thể đoán được là những dữ liệu xem được trong file pcap sẽ có liên quan đến đoạn mã C# trên

Giờ sẽ đi giải mã từng phần một, đầu tiên là đoạn mã HTML có được do server respone, có tất cả 4 luồng chứa đoạn mã HTML.

Nhưng trước hết phải tìm được tagHex để ánh xạ các thẻ trong HTML thành kí tự, lướt qua 1 lượt thì mình đã thấy

![image](https://github.com/user-attachments/assets/f8b0dd16-e7fa-4b38-b765-b9fbc06818c4)

Và đây là script Python giải mã: 
```Python
import base64
import re

tag_hex = {
    "cite": "0",
    "h1": "1",
    "p": "2",
    "a": "3",
    "img": "4",
    "ul": "5",
    "ol": "6",
    "button": "7",
    "div": "8",
    "span": "9",
    "label": "a",
    "textarea": "b",
    "nav": "c",
    "b": "d",
    "i": "e",
    "blockquote": "f"
}

def hex_string_to_bytes(hex_str):
    bytes_data = bytes.fromhex(hex_str)
    return bytes_data.decode('ascii')

def decode_data(data):
    body = re.search(r"<body>(.*?)</body>", data, re.S)
    if not body:
        return "No <body> tag found."
    body_text = body.group(1)
    string_builder = []
    for match in re.finditer(r"<(\w+)[\s>]", body_text):
        tag = match.group(1)
        if tag in tag_hex:
            string_builder.append(tag_hex[tag])
    return hex_string_to_bytes(''.join(string_builder))

html_data = '''
Đoạn mã HTML
'''

decoded_output = decode_data(html_data)
print("Decoded Output:", decoded_output)
```
 Đầu tiên là eq 0

 ![image](https://github.com/user-attachments/assets/06fb8e5e-cfd8-4da7-aa23-36a0c593aec9)

eq 2 không có gì đặc biệt, đến với eq4 thì thấy được part 1 của flag

![image](https://github.com/user-attachments/assets/c4f9e556-9260-4517-889b-fbe0a824ff14)

```
Part1: HTB{Th4ts_d07n37_
```

eq 6 cũng không có gì, vậy là đã khai thác hết mã HTML để tìm flag

Giờ đến với các luồng stream còn lại kết hợp với các hàm sau trong C# để viết script giải mã

![image](https://github.com/user-attachments/assets/2ec83a57-a91e-415e-8c6b-df6025a6b668)

![image](https://github.com/user-attachments/assets/11e779ad-a9df-40d1-a887-ad3289f117fb)

Sẽ trích xuất thông tin ở phần feedback do server response và đây là script python giải mã

``` Python
import base64
def decode_base64(encoded_str):
    temp = ""
    for word in encoded_str.split(" "):
        if word:
            temp += word[0]
    try:
        while len(temp) % 4:
            temp += '='
        print(base64.b64decode(temp).decode('utf-8'))
    except Exception as e:
        print("Error decoding base64:", e)


feedback = ""
payload = feedback.split("\n")

for i in range(len(payload)):
    decode_base64(payload[i])
```
Đây là eq 1: 

![image](https://github.com/user-attachments/assets/a5ef300b-2f16-4b47-bd08-4c99e828975f)

Kết quả:

![image](https://github.com/user-attachments/assets/c16ac0cc-d3b5-4450-84f2-b6460d8234b6)

Đây là eq 3 khá dài:

![image](https://github.com/user-attachments/assets/a4948a6c-562c-4dd1-9cae-96fd8a7ddd4d)

Kết quả:

```
Host Name:                 WINDOWS-INSTANC

OS Name:                   Microsoft Windows Server 2016 Datacenter

OS Version:                10.0.14393 N/A Build 14393

OS Manufacturer:           Microsoft Corporation

OS Configuration:          Standalone Server

OS Build Type:             Multiprocessor Free

Registered Owner:          N/A

Registered Organization:   N/A

Product ID:                00376-40000-00000-AA673

Original Install Date:     5/7/2024, 9:06:45 AM

System Boot Time:          5/7/2024, 9:06:03 AM

System Manufacturer:       Google

System Model:              Google Compute Engine

System Type:               x64-based PC

Processor(s):              1 Processor(s) Installed.

                           [01]: Intel64 Family 6 Model 79 Stepping 0 GenuineIntel ~2200 Mhz

BIOS Version:              Google Google, 3/27/2024

Windows Directory:         C:\Windows

System Directory:          C:\Windows\system32

Boot Device:               \Device\HarddiskVolume2

System Locale:             en-us;English (United States)

Input Locale:              en-us;English (United States)

Time Zone:                 (UTC+00:00) Monrovia, Reykjavik

Total Physical Memory:     4,092 MB

Available Physical Memory: 2,174 MB

Virtual Memory: Max Size:  5,116 MB

Virtual Memory: Available: 3,259 MB

Virtual Memory: In Use:    1,857 MB

Page File Location(s):     C:\pagefile.sys

Domain:                    WORKGROUP

Logon Server:              \\WINDOWS-INSTANC

Hotfix(s):                 9 Hotfix(s) Installed.

                           [01]: KB5036609

                           [02]: KB4049065

                           [03]: KB4486129

                           [04]: KB4524244

                           [05]: KB4562561

                           [06]: KB4589210

                           [07]: KB5012170

                           [08]: KB5037016

                           [09]: KB5036899

Network Card(s):           1 NIC(s) Installed.

                           [01]: Google VirtIO Ethernet Adapter

                                 Connection Name: Ethernet

                                 DHCP Enabled:    Yes

                                 DHCP Server:     169.254.169.254

                                 IP address(es)

                                 [01]: 10.142.0.4

                                 [02]: fe80::a4cf:f41b:453c:5afc

Hyper-V Requirements:      A hypervisor has been detected. Features required for Hyper-V will not be displayed.
```

Và đây là eq 5:

![image](https://github.com/user-attachments/assets/37c7590f-91cf-4d47-85c1-4f677b5fe53c)

Có được part2 flag luôn

![image](https://github.com/user-attachments/assets/c6bc190a-f2c2-4fc4-b919-66f8697830b5)

```
Flag: HTB{Th4ts_d07n37_h77P_s73417hy_revSHELL}
```











