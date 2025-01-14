# _TRUE SECRETS_
![image](https://github.com/4venger/CTF-writeups/blob/main/HTB/Screenshot%202025-01-15%20031111.png)
Chal này được tag easy nhưng với trình độ của mình hiện tại thì mình thấy khó vl
## _Solution_
Tải tệp zip về giải nén thì được 1 file .raw

![image](https://github.com/user-attachments/assets/549c1ef3-aff8-4cf4-94b0-8e01868e9213)

Theo mô tả của bài _"...Fortunately, our unit was able to raid the home of the leader of the APT group and take a memory capture of his computer while it was still powered on..."_ thì mình đoán thử thách này liên quan đến **MEMORY DUMP**

Mình dùng **volatility3** với plugin là windows.pstree để xem các tiến trình

![image](https://github.com/user-attachments/assets/f9b50a22-9aef-4766-8353-6ee47772a52e)

Xem qua một lượt thì mình thấy toàn những tiến trình quen thuộc của windows như **svchost.exe**, **expoler.exe**, ... và chúng đều không nằm ở các đường dẫn lạ

Lướt xuống cuối thì thấy 2 tiến trình khá lạ là *True Crypt.exe* và *7zFM.exe*

![image](https://github.com/user-attachments/assets/7baf7c5b-fdb8-4298-b803-963b1a9271ab)

Nên mình thử dùng plungin **windows.dumpfiles** để trích xuất hết các file có trong đó với PID của thằng ***True Crypt.exe*** là 2128

![image](https://github.com/user-attachments/assets/9c9df0ff-261f-455d-a0ee-4fc64eacf6bc)

Kết quả là chả có gì ngoài mấy file dll.img

![image](https://github.com/user-attachments/assets/39328d9f-f9b6-4af8-9337-c73359c4bced)

Mình thử strings 1 2 file, thì đều là file thực thi

![image](https://github.com/user-attachments/assets/32915582-8999-4f78-9e97-e6fbda6d9325)

Mình thử tiếp với PID của thằng **7zFM.exe** là 2176

![image](https://github.com/user-attachments/assets/11545599-9d50-4b30-85e3-78915d54e32b)

Kết quả có vẻ khả quan hơn khi mình nhìn thấy có 2 file .zip

![image](https://github.com/user-attachments/assets/74910e5b-8420-439a-a7d1-90a7216cbfca)

Thử unzip 1 cái xem sao

![image](https://github.com/user-attachments/assets/3ca72934-c902-45ca-b7eb-d490ff034326)

Lòi ra 1 file .tc, đến đây là mình méo biết làm thế nào nữa r nhưng theo thói quen khi gặp file lạ thì mình hay xem hex file rồi hỏi chat gpt :))

![image](https://github.com/user-attachments/assets/da372265-189e-4404-8f7b-88851aa16e6d)

Lần đầu mình nghe đến cái loại này, nhớ lại thì lúc nãy có làm việc với file True crypt.exe nên mình thấy cũng khá hợp lí

Và chat GPT cũng chỉ mình cách để làm việc với file đó luôn <3

![image](https://github.com/user-attachments/assets/f43ea845-502d-47c3-8581-e93bbac1ccbc)

Mình search gg VeraCrypt rồi tải về và đọc theo hướng dẫn của chat gpt tiếp thui

![image](https://github.com/user-attachments/assets/7771741c-ef4a-46f8-a3e0-f0ad9f0b78d9)

Đến đây thì cần pass :)) và rồi mình lại mắc kẹt, thực sự không biết làm gì tiếp theo và rồi sau rất nhiều thời gian trao đổi với chat gpt thì mình cũng có được :)))

![image](https://github.com/user-attachments/assets/3e4ff448-bbec-4243-9b2b-b871650cdefe)

Nhưng nó lại hướng dẫn với vol2 :)) nên mình dùng vol3 -h để xem cách biểu diễn plugin 

![image](https://github.com/user-attachments/assets/cc212681-37a3-4950-9f38-d2137a830958)

Và đã có kết quả 

![image](https://github.com/user-attachments/assets/588f9296-e239-43c2-82c8-df9b0f70d8b3)

Mình nhập pass

![image](https://github.com/user-attachments/assets/7625d0a6-3645-43f2-b56b-fe5fea0c8ec0)

Nhưng phiên bản mình tải lại không hỗ trợ

![image](https://github.com/user-attachments/assets/debe148e-1c0f-4330-bf07-9b91f8f2e9b0)

Mình lại ngồi mày mò, sau một hồi thì thấy được 1 bài viết về TrueCrypt support

![image](https://github.com/user-attachments/assets/79158bc2-350f-4b34-a128-aee280efa3e1)

Ở bài viết trên có nói là chỉ hỗ trợ các phiên bản 1.0f nên mình tìm và cài lại phiên bản đó

![image](https://github.com/user-attachments/assets/25085a8d-0e0c-4d13-ad35-d16194489ca8)

Mở ổ đĩa mới tạo ra thì thấy có 1 file .cs và một folder seesion chứa 3 file. xem file .cs trước

![image](https://github.com/user-attachments/assets/543bb8dd-5b7c-42b2-a6c9-e1423bbb2f9a)

```
using System;
using System.IO;
using System.Net;
using System.Net.Sockets;
using System.Text;
using System.Security.Cryptography;

class AgentServer {
  
    static void Main(String[] args)
    {
        var localPort = 40001;
        IPAddress localAddress = IPAddress.Any;
        TcpListener listener = new TcpListener(localAddress, localPort);
        listener.Start();
        Console.WriteLine("Waiting for remote connection from remote agents (infected machines)...");
    
        TcpClient client = listener.AcceptTcpClient();
        Console.WriteLine("Received remote connection");
        NetworkStream cStream = client.GetStream();
    
        string sessionID = Guid.NewGuid().ToString();
    
        while (true)
        {
            string cmd = Console.ReadLine();
            byte[] cmdBytes = Encoding.UTF8.GetBytes(cmd);
            cStream.Write(cmdBytes, 0, cmdBytes.Length);
            
            byte[] buffer = new byte[client.ReceiveBufferSize];
            int bytesRead = cStream.Read(buffer, 0, client.ReceiveBufferSize);
            string cmdOut = Encoding.ASCII.GetString(buffer, 0, bytesRead);
            
            string sessionFile = sessionID + ".log.enc";
            File.AppendAllText(@"sessions\" + sessionFile, 
                Encrypt(
                    "Cmd: " + cmd + Environment.NewLine + cmdOut
                ) + Environment.NewLine
            );
        }
    }
    
    private static string Encrypt(string pt)
    {
        string key = "AKaPdSgV";
        string iv = "QeThWmYq";
        byte[] keyBytes = Encoding.UTF8.GetBytes(key);
        byte[] ivBytes = Encoding.UTF8.GetBytes(iv);
        byte[] inputBytes = System.Text.Encoding.UTF8.GetBytes(pt);
        
        using (DESCryptoServiceProvider dsp = new DESCryptoServiceProvider())
        {
            var mstr = new MemoryStream();
            var crystr = new CryptoStream(mstr, dsp.CreateEncryptor(keyBytes, ivBytes), CryptoStreamMode.Write);
            crystr.Write(inputBytes, 0, inputBytes.Length);
            crystr.FlushFinalBlock();
            return Convert.ToBase64String(mstr.ToArray());
        }
    }
}
```
Đọc qua thì có thể thấy đây là kiểu mã hóa DES với key và iv cho sẵn như ở trên

Với 3 file tiếp thì mình thấy những kí tự rất quen nhất là có dấu "=" :)) Khả năng là mã hóa base 64

```
wENDQtzYcL3CKv0lnnJ4hk0JYvJVBMwTj7a4Plq8h68=
M35jHmvkY9WGlWdXo0ByOJrYhHmtC8O0rZ28CviPexkfHCFTfKUQVw==
hufGZi+isAzspq9AOs+sIwqijQL53yIJa5EVcXF3QLLwXPS1AejOWfPzJZ/wHQbBAIOxsJJIcFq0+83hkFcz+Jz9HAGl8oDianTHILnUlzl1oEc30scurf41lEg+KSu/6orcZQl3Bws=
6ySb2CBt+Z1SZ4GlB7/yL4cOS/j1whoSEqkyri0dj0juRpFBc4kqLw==
U2ltlIYcyGYnuh0P+ahTMe3t9e+TYxKwU+PGm/UsltpkanmBmWym5mDDqqQ14J/VSSgCRKXn/E+DKaxmNc9PpPOG1vZndmflMUnuTUzbiIdHBUAEOWMO8wVCufhanIdN56BhtczjrJS5HRvl9NwE/FNkLGZt6HQNSgDRzrpY0mseJHjTbkal6nh226f43X3ZihIF4sdLn7l766ZksE9JDASBi7qEotE7f0yxEbStNOZ1QPDchKVFkw==

```
Mình lên cyberchef rồi thực hiện lần lượt để decode, với key và iv khi decode DES là 

```
string key = "AKaPdSgV";
string iv = "QeThWmYq";
```
2 file đầu decode thì không thấy j, đến file thứ 3 thì flag đã xuất hiện 

![image](https://github.com/user-attachments/assets/3d8f913c-ac68-420e-952c-233501efc148)

```
FLAG: HTB{570r1ng_53cr37_1n_m3m0ry_15_n07_g00d}
```






