Started with a nmap scan
```
22 OpenSSH 7.9p1
80 Apache 2.4.38
```
Going to the site we just see a picture checking if there is a robots file and there is it goers to a `/find_me` which has a html file in it that says `vegeta-1.0` so im going to run a gobuster scan on the main part of the site since we can see the full directory listing for this folder
```
/img
.index.html
/login.php
/image
/admin
/manual
/robots.txt
/find_me/
```
did not find much after this so going back to `find_me` and looking at the source there is some `base64`  at the bottom of the page and going to cyberchef its a QR code that has `password:topshellv` in the qr code but we have no login page both the login pages have `0 bytes` but this seemed to be a rabbit hole so went a google a list of characters names and tried a gobuster with this and got  a hit `/bulma` and has a `.wav` file and it sounds like morse code so we can use a audio converter and we get another login `trunks:u$3r` and we can SSH into this user and we have stuff in are `.bash_history` file showing it adding a user to the `/etc/passwd` file and gives us the password as well looking into this file we have write permissions for this file and can just run the same commands as the history file to get root  