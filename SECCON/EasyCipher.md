This chall was quite simple!

We were given a string containing some binary, octal, decimal and hexadicimal values.

Here's is my quick python's script used to decipher the differents values.


```python
#!/usr/bin/python

c = '87 101 108 1100011 0157 6d 0145 040 116 0157 100000 0164 104 1100101 32 0123 69 67 0103 1001111 \
     1001110 040 062 060 49 064 100000 0157 110 6c 0151 1101110 101 040 0103 1010100 70 101110 0124 \
     1101000 101 100000 1010011 1000101 67 0103 4f 4e 100000 105 1110011 040 116 1101000 0145 040 \
     1100010 0151 103 103 0145 1110011 0164 100000 1101000 0141 99 6b 1100101 0162 32 0143 111 1101110\
     1110100 101 0163 0164 040 0151 0156 040 74 0141 1110000 1100001 0156 056 4f 0157 0160 115 44 040\
     0171 1101111 117 100000 1110111 0141 0156 1110100 32 0164 6f 32 6b 1101110 1101111 1110111 100000\
     0164 1101000 0145 040 0146 6c 97 1100111 2c 100000 0144 111 110 100111 116 100000 1111001 6f 117\
     63 0110 1100101 0162 0145 100000 1111001 111 117 100000 97 114 0145 46 1010011 0105 0103 67 79\
     1001110 123 87 110011 110001 67 110000 1001101 32 55 060 100000 110111 0110 110011 32 53 51 0103\
     0103 060 0116 040 5a 0117 73 0101 7d 1001000 0141 1110110 1100101 100000 102 0165 0156 33'
     
flag = ""
for _ in c.split(' '):
  if len(_) == 2 and _[1:].isalpha(): #HEX
    flag += _.decode('hex')
  if (len(_) == 2 and not _[1:].isalpha()) or (len(_) == 3 and int(_[0]) != 0): #DEC
    flag += chr(int(_))
  if len(_) == 4 or (len(_) == 3 and int(_[0]) == 0) : #OCT
    flag += chr(int(_, 8))
  if len(_) > 4: #BIN
    flag += binascii.unhexlify('%x' % int(_,2))
print flag
```

I got this result:<br>

**Welcome to the SECCON 2014 online CTF.The SECCON is the biggest hacker contest in Japan.Oops, you want to know the flag, don't you?Here you are.SECCON{W31C0M 70 7H3 53CC0N ZOIA}Have fun!**

Hence, the flag was **SECCON{W31C0M 70 7H3 53CC0N ZOIA}**


Enjoyed :)

