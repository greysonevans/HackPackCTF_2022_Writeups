# Self-Hosted-Crypto
***Disclaimer: I solved this challenge after the CTF challenges became unavailable, though without a writeup.***
## Details
- Category: Reverse Engineering
- Flag: `flag{A_b4d_Id34!1!}`
- Synopsis:
	- Reverse engineer `encrypter` binary to understand how the program scrambles text.
	- Write a script to convert the encrypted flag to its original state via a ROT-13 cipher.
## Writeup
The challenge provides two files, `encrypter` (a go-compiled binary), and the `encrypted` flag.

Opening up `encrypter` in IDA-Pro Home, I viewed the `main_main` function, then decompiled the function using IDA-Pro's `generate pseudocode` feature using the `f5` shortcut.

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220412110519.png)
![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220412110730.png)

From inspecting the generated code, it appears that the binary's encryption process involves running a for-loop for each character in a file appending `13` to the character's decimal value, then writing the conversions to `encrypted`.

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220412111133.png)

To test whether this theory works, I passed a text file to the binary containing "abc", then attempted to reverse the encryption process.

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220412111346.png)

Sure enough, the encrypter functions as a classic ROT-13 cipher!

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220412111449.png)

While inputting the encrypted flag to a ROT-13 cipher may seem convenient, much of the characters in the file are not readable, meaning that some python scripting is required to seamlessly solve the challenge.

![image](https://github.com/greysonevans/HackPackCTF_2022_Writeups/blob/main/images/Pasted%20image%2020220412111710.png?raw=true)

Using the output from hexdump, I appended the contents to a python file and converted the encrypted string back to it's original form, resulting in the flag!

Doing so required:
 - Appending the hexadecimal values to a python list.
 - Converting the hexadecimal values to decimal, then subtracting 13 from the value.
 - Adding the subtracted values to a list containing the flag.
 - Printing the flag in ASCII format.

![image](https://github.com/greysonevans/HackPackCTF_2022_Writeups/blob/main/images/Pasted%20image%2020220412111828.png?raw=true)

Below, includes my solution for the challenge, `solve.py`.


```python
#!/usr/bin/env python3

import ast

encoded_flag_hex = "73 79 6e 74 88 4e 6c 6f 41 71 6c 56 71 40 41 2e 3e 2e 8a".split(" ")
encoded_flag_decimal = []
flag = []

# Convert hexadecimal characters to decimal
for i in range(len(encoded_flag_hex)):
	encoded_flag_decimal.append(ast.literal_eval("0x" + encoded_flag_hex[i]))

# Remove 13 from each decimal value.
for i in encoded_flag_decimal:
	flag.append(i-13)

# Convert decimal notation to ASCII
for i in range(len(flag)):
	flag[i] = chr(flag[i])

print(''.join(flag))
```
