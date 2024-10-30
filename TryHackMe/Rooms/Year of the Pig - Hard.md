Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29 Ubuntu
```
Started a gobuster scan
```
/login.php
index.html
/admin
/assets
/css
/js
/api
```
The only thing we found was a login page for admin and when we try to login to the page its gives us a hint to what the password could be it says `passwords should be a memorable word followed by two numbers and a special character` so I took some words from the site and made a python script to make a wordlist but I also noticed the passwords when login into the app are hashed with MD5 so I went to burp and bruteforce the login and since its marco sight I tested that as the username and we got the password of `sav.....` now that we can login in to the app but I could not get much done played around with command injection for a while until I just tried the same login on ssh and got logged in looking around the server a bit I found a `admin.db` file but `www-data` owns it but we own all the other files so I put a php revshell into the site and got a call back as `www-data` and was able to get the `admin.db` file back onto my machine were I could dump it with sqlite3 and got some hashes for the user `curtis` got the password for the user now its was hashed in MD5 also `Donald.....` first I ran` sudo -l` and we can edit a `config.php` file with sudoedit so I made the `config.php` file now what we could do is link the shadow file to the `config.php `or we can use PwnKit to get root 