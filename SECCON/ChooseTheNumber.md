
Another quick and easy chall.
```python
#!/usr/bin/python                                                                                                                                                                            

import socket
import time

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

Greetz to [fr0g](https://twitter.com/fr0gSecurity)
