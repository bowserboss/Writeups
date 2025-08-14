Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
```
Going to the website its apache default page so going to run a gobuster scan on the host
```
/javascript
/robots.txt
/election
/phpinfo.php
```
The `robots` file has a list of 4 folders and only one is a real folder `election` running another scan inside the `election` folder we find `card.php` that is full of `01110001` and we decoded it 2 times and decodes to a login `1234:Zxc123!@#` and this gets us into the admin page `/election/admin` now were signed in if you go to settings and click on system info and then logging we can read the logs and get the password for the user `love` `love:P@$$w0rd@123` running linpeas exploited root with `PwnKit` 