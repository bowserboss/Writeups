Started with a nmap scan
```
22 OpenSSH 7.2p2
80 Apache 2.4.18
```
Going to the webserver just a welcome message if we look at the source code for the site there is a folder called `/nibbleblog/` so going there its a blog with no posts googling the folder I found its a cms for blog host going to do a gobuster scan of the site 
```
/sitemap.php
/content
/themes
/feed.php
/index.php
/admin.php
/update.php
/README
```
looking at the readme file its running version `4.0.3` which has a exploit for it but we need creds first if we go to `/admin/` we can view all the code for the site and going to `admin.php` is the admin login page after some testing and googling I got a login `admin:nibbles` and now we have a account now we can use the exploit to get a shell on the host and we get a call back as `nibbler` and when we run `sudo -l` there is a sh script we can run as root called `monitor.sh` and when we unzip it on the host the permissions it sets we can modify the script to output the root flag to the tmp folder and we can get the flag that way 