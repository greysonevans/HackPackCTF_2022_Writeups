# Shopkeeper 1
## Details
- Category: Reverse Engineering
- Flag: `flag{b4s364_1s_s0_c3wl_wh0_kn3w_you_c0u1d_do_th15}`
- Synopsis:
	- Connect to `cha.hackpack.club:10992` via netcat.
	- Decode Base64-encoded content to an output file, and read the flag using the `strings` command.
## Writeup
Beginning the challenge, I used the command `nc cha.hackpack.club 10992` to interact with the CTF. After initiating a connection, a long string of text is outputted before an interactive game appears. Based on the characteristics of the string (i.e., equal signs at the end of the string), I knew the text was likely Base64 encoded.

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220410152521.png)
![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220410152544.png)

Then, I decoded the Base64 string to further inspect it's content, revealing an ELF 64-bit binary. Upon executing the file, the same shopkeeping game appeared!

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220410153620.png)

Afterward, I used the `strings` command to read printable characters in the binary, revealing the flag!

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220410153958.png)
