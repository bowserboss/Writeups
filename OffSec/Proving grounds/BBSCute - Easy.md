Started with a nmap scan
```
22 OpenSSH 7.9p1
80 Apache 2.4.38
88 nginx 1.14.2
110 Courire
995 pop3
```
Starting with a gobuster scan on port `80` we had to add `.php` to the search and now we get better results 
```
/index.php
```
going to the index page we get `cutenews 2.1.2` and we can make a user but we have to run this in caido because the captua does not load on the page but we only have access as `Commenter` so going back to google to look for exploits I found a couple but one worked `CVE-2019-1147` its a RCE and we have to edit this exploit and change the url and it now works and we get a user as `www-data` checking for SUID bit sets we find `/usr/bin/hping3` and we can use this to get to root by running the program and then `/bin/bash -p` we now root user 