Started with a nmap scan 
```
21 ProFTPD
80 Apache 2.4.48 PHP 8.0.7 Perl 5.32.1
443 Apache 2.4.48 PHP 8.0.7 Perl 5.32.1
3306 mysql
```
Starting with the FTP it has anonymous login enabled and there is a folder called `pub` and there is a file called `message.txt` and another folder called `.secret` with two more files called `flag.txt` and `keep_in_mind.txt` 

Now going to the webserver 
Going to start a gobuster scan
```
/login.html
/login.php
/css
/imgs
/js
/api
/logout.php
/logs
/session.php
/phpmyadmin
/echo.php
```
 There is a login page and after some digging in the js files I found a password gen script and says its a pwd gen for `Daedalus` and got the password from it `g2e5.....` and this logs us into the website the search query is injectable to 4 different types of sql injections found some usernames and passwords in the `labyrinth` database after looking around the database I did not find anything else besides the logins so I logged into the admin account `M!n0taur:amin....` got the 2ed flag and we go to the secret page which is `echo.php` and it reflects are text that we enter but to break out of this and get RCE on the host we can just do this ``ls``  so now time to get reverse shell first step take the revshell and base64 encode it and leave off the `=` sign since its on the blacklist and then enter this command ``echo <base64> | base64 -d | tee /tmp/shell.sh`` and then to run it ``/bin/bash /tmp/shell.sh`` and now we got a shell as daemon got root with pwnkit
