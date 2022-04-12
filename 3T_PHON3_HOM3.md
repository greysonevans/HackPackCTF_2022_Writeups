# 3T PHON3 HOM3
## Details
- Category: Reverse Engineering
- Flag: `flag{resourceful_find!}`
- Synopsis:
	- Download `maybeaninvasionapp.apk`
	- Run the `strings` command and `grep` for the flag.
## Writeup
Upon downloading `maybeaninvasionapp.apk`, I ran `strings maybeaninvasionapp.apk | grep -i flag` to read the flag.

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/rev21.png)
