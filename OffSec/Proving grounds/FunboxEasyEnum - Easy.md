Started with a nmap scan
```
22 OpenSSH 7.6p1 Ubuntu
80 Apache 2.4.29
```
The nmap scan shows its a default apache page going to start a gobuster scan
```
/javascript
/phpmyadmin version 4.6.6deb5
/robots.txt
/mini.php
```
We have found `mini.php` file which is a `Zerion Mail Shell` was trying to upload a rev shell but found we can just change the paths with this shell flag outside of the `html` folder now to get reverse shell we need to chmod the shell to `4607` looking at the passwd file there is a hash for oracle and it cracked to `hiphop` but this was nothing going back to the other user to check the `config-db.php` file for phpmyadmin we get some more creds user `phpmyadmin` password `tgbzhnujm!` time to also test this password on all the users 