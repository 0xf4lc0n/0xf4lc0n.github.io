---
layout: default
---

# ZED-Crackme made by zed-zahir
[You can find it here](https://crackmes.one/crackme/5ecd7cc133c5d449d91ae641)

When I unpacked the archive and tried to run crackme, a segmentation fault error has occured.

```plain
# ./ZED-Crackme-x64.bin 
[1]    11303 segmentation fault (core dumped)  ./ZED-Crackme-x64.bin
```

By using the string command I recognized that the file was packed with UPX:

```plain
...
$Info: This file is packed with the UPX executable packer http://upx.sf.net $
$Id: UPX 3.94 Copyright (C) 1996-2017 the UPX Team. All Rights Reserved. $
...
```

After unpacking the file it ran properly:

```plain
# upx -d ZED-Crackme-x64.bin 
# ./ZED-Crackme-x64.bin
***********************
**      rules:       **
***********************

* do not bruteforce
* do not patch, find instead the serial.

enter the passphrase: test
try again
```

The program displayed the rules and waited for password.

I opened it in IDA. Here we get a first part of a main function:

![](../assets/images/crackme/zed_crackme/main_stage_1.png)

After printing rules program saves a null terminated string into memory:

```nasm
mov     rax, 2073692073696854h
mov     rdx, 657320706F742061h
mov     [rbp+var_30], rax
mov     [rbp+var_28], rdx
mov     rax, 7865742074657263h
mov     rdx, 67617373656D2074h
mov     [rbp+var_20], rax
mov     [rbp+var_18], rdx
mov     [rbp+var_10], 2165h
mov     [rbp+var_E], 0
```

Let's call this string "message". 

So "message" contains "This is a top secret text message!".

Next the program checks if it has been run in the virtual machine. If so, the program is terminated otherwise execution proceeds to the next stage.

![](../assets/images/crackme/zed_crackme/main_stage_2.png)

The "message" address is loaded to rax register then copied to rsi and rot function is called. The process is then repeated.

```nasm
lea     rax, [rbp+var_30]
mov     rsi, rax
mov     edi, 0Dh
call    rot
lea     rax, [rbp+var_30]
mov     rsi, rax
mov     edi, 0Dh
call    rot
```

ROT13 is its own inverse so "message" is firstly encoded and then decoded.

```plain
"This is a top secret text message!" => rot13 => "Guvf vf n gbc frperg grkg zrffntr!"
"Guvf vf n gbc frperg grkg zrffntr!" => rot13 => "This is a top secret text message!"
```

However "message" won't be used any more in the program and it doesn't have any influence on serial key generation.

Next the another string is saved into memory. I named it "password_base".

```nasm
mov     rax, 4145443332694841h
mov     rdx, 464F434645454244h
mov     [rbp+var_90], rax
mov     [rbp+var_88], rdx
mov     [rbp+var_80], 4546h
mov     [rbp+var_7E], 45h
```

The "password_base" contains "AHi23DEADBEEFCOFFEE" and this string will be used to create serial key. In the next lines program takes input from user and calls ptrace in order to check if it's currently debugging. If so the program is terminated otherwise it will start creating a serial key.

Here the program creates the first five letters of the key:

![](../assets/images/crackme/zed_crackme/main_stage_3.png)

The folowing listing listing explains that part:

```nasm
; rbp+var_90 => "AHi23DEADBEEFCOFFEE"
movzx   eax, byte ptr [rbp+var_90]      ; eax = 0x41 'A'
xor     eax, 2                          ; eax = 0x43 'C'
mov     [rbp+s1], al                    ; serial_key[0] = 'C'
movzx   eax, byte ptr [rbp+var_90+3]    ; eax = 0x32 '2'
sub     eax, 0Ah                        ; eax = 0x28 '('
mov     [rbp+var_4F], al                ; serial_key[1] = '('
movzx   eax, byte ptr [rbp+var_90+2]    ; eax = 0x69 'i'
add     eax, 0Ch                        ; eax = 0x75 'u'
mov     [rbp+var_4E], al                ; serial_key[2] = 'u'
movzx   eax, byte ptr [rbp+var_90+2]    ; eax = 0x69 'i'
mov     [rbp+var_4D], al                ; seial_key[3] = 'i'
movzx   eax, byte ptr [rbp+var_90+1]    ; eax = 0x48 'H'
add     eax, 1                          ; eax = 0x49 'I'
mov     [rbp+var_4C], al                ; serial_key[4] = 'I'
```

Then program sets index to 5 and jumps to loop.

```nasm
mov     [rbp+var_9C], 5
```

![](../assets/images/crackme/zed_crackme/main_stage_4.png)

Here we got a loop condition:

```nasm
cmp     [rbp+var_9C], 12h               ; proceed until index isn't equal to 18
jle     short loc_1514
```

In loop program takes each character from "base_password" (starting from 6th character), subtracts 1 from character's ascii code and then puts the new character into memory (in serial_key array). 

Here is a simple Python script that creates the required serial key:

```python
password_base = "AHi23DEADBEEFCOFFEE"
decoded_password = []

decoded_password.append(chr(ord(password_base[0]) ^ 2))
decoded_password.append(chr(ord(password_base[3]) - 0x0A))
decoded_password.append(chr(ord(password_base[2]) + 0x0C))
decoded_password.append(password_base[2])
decoded_password.append(chr(ord(password_base[1]) + 1))

for c in password_base[5:]:
    decoded_password.append(chr(ord(c) - 1))

print("Decoded serial key: ", "".join(decoded_password))
```

And that's all. When the program creates a serial key, it compares the serial key with the data entered by user. If both strings are the same, crackme is resolved.

```plain
lea     rdx, [rbp+s2]
lea     rax, [rbp+s1]
mov     rsi, rdx        ; s2
mov     rdi, rax        ; s1
call    _strcmp
test    eax, eax
jnz     short loc_156A   ; bad serial key
lea     rdi, aYouSucceed ; "you succeed!!"
call    _puts
jmp     short loc_1576   ; end program
```

```plain
# ./ZED-Crackme-x64.bin 
***********************
**      rules:       **
***********************

* do not bruteforce
* do not patch, find instead the serial.

enter the passphrase: C(uiICD@CADDEBNEEDD
you succeed!!
```

Used tools:
* Ida Freeware 7.0
* GDB