
Another quick and easy chall :)

The question we wera given was: **nc number.quals.seccon.jp 31337**

After trying some with [nc](http://nc110.sourceforge.net/) we figured out that we have to solved it by writing a prog that would be able to choose among some randoms numbers the min or max.

![alt text](http://saxx.swordarmor.fr/CTF/prog100_nc.png "Netcat")

Here is the python exploit:

```python
#!/usr/bin/python

import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(("number.quals.seccon.jp",31337))

while True:
    r = s.recv(4096)
    print r+'\n'
    if "Congratulations!" in r:
	print "Done"
        print s.recv(4096)
	exit(0)
    r = r.split("\n")
    x = [int(z) for z in r[0].split(', ')]
    if "max" in r[1]: r= max(x)
    else: r=min(x)
    print r
    s.send(str(r)+'\n')
```

After some hits, we got the flag:

![alt text](http://saxx.swordarmor.fr/CTF/prog100_flag.png "Flag")

Greetz to [fr0g](https://twitter.com/fr0gSecurity)

Have fun :)
