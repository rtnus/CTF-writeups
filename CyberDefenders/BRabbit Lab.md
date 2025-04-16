![image](https://github.com/user-attachments/assets/2831c018-3aa2-4eb4-af1a-81c266ada697)

### Description
> You are an investigator assigned to assist Drumbo, a company that recently fell victim to a ransomware attack. The attack began when an employee received an email that appeared to be from the boss. It featured the company’s logo and a familiar email address. Believing the email was legitimate, the employee opened the attachment, which compromised the system and deployed ransomware, encrypting sensitive files. Your task is to investigate and analyze the artifacts to uncover information about the attacker.

Thông tin thêm về con mã độc vào cuối năm 2017 tại [đây](https://www.sitelock.com/blog/what-is-bad-rabbit-ransomware/)

Giờ cùng đi phân tích con `ransomware` này nhé!

## Solution

>Q1. The phishing email used to deliver the malicious attachment showed several indicators of a potential social engineering attempt. Recognizing these indicators can help identify similar threats in the future.
What is the suspicious email address that sent the attachment?

Mở file `eml` bài cho là có đáp án

![image](https://github.com/user-attachments/assets/12350b11-9ca7-42f7-a492-041e0b2626d9)

`Answer: theceojamessmith@Drurnbo.com`

>Q2. The ransomware was identified as part of a known malware family. Determining its family name can provide critical insights into its behavior and remediation strategies.
What is the family name of the ransomware identified during the investigation?

Đính kèm với email trên là một file `.exe` lưu về rồi đem lên virustotal

![image](https://github.com/user-attachments/assets/2f2ccb41-5133-498f-8428-fe106a66b9d2)

`Answer: BadRabbit`

