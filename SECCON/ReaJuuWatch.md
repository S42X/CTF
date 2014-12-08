Hey everyone!

This challenge was introduced by a sort of [dating game](http://reajuu.pwn.seccon.jp/).
There is a button to create a new account. It is generated randomly.

After a few question, the website will give you a score. According to them, girls don't like going to seccon, because my first score was -269 (girl-what?)

Anyway, once you beat the game, if you look closely, you can see an ajax request sent to their servers to retrieve your score.
The page they retrieve is similar to [this one](http://reajuu.pwn.seccon.jp/users/chk/10761).

Bummer, it contains a username, and a password. Which happens to be my credentials.

Page for [user 1](http://reajuu.pwn.seccon.jp/users/chk/1) gives you the following response :

`{"username":"rea-juu","password":"way_t0_f1ag","point":99999}`

Now that you have the admin's password, all you have to do to get the flag is to login with their credentials, and beat the game again.

```
GAME OVER
Your score is 99999point!
Logout
SECCON{REA_JUU_Ji8A_NYAN}
```

Conclusion: It's ok if a girl doesn't like you. You can always find a way to her flag. ;)
(Ok, I'm joking. Please, don't call the cops.)
