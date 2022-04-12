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
