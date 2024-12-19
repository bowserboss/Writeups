Started with a nmap scan
```
22 OpenSSH 8.4p1
80 nginx 1.18.0
```
Nmap shows the webserver is redirecting to a url `http://pilgrimage.htb/` we can make a account on the site there is no sql injection in the register going to start a gobuster scan
```
/index.php
/login.php
/register.php
/assets
/vendor
/tmp
/dashboard.php
/.git/
```
there is a `.git` folder we can use gittools to dump the git folder and we can run `git checkoput -- .` and see its running image magick version `7.1.0-49` and is vulnerable to `CVE-2022-44268` we can use this to read files on the system and we use the website to convert them for us and then we download them and run `identify --verbose image.png` and there is some hex in there and use cyberchef to convert it and there is a user `emily` since we have the source code for the site we can try and get the sqlite database and see what's in it `/var/db/pilgrimage` and then we can convert it the same way and now we have the login for `emily:abigchonkyboi123` if you run `ps aux` you can see root is running  a bash script and looking at the bash script its exacuteing binwalk in the `/var/www/pilgrimage.htb/shrunk` and there is a CVE for this version of binwalk `CVE-2022-4510` and we can make a exploit for a reverse shell and drop the image in that folder and get a call back as the root user 