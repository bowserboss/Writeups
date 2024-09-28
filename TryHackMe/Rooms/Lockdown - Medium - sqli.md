Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 24.29
```
When going to the webhost it directs us to a domain called `contacttracer.thm` when going to the site its a coronavirus contact tracer and its asking for a code and has a link to a admin panel so time to start a gobuster 
```
/index.php
/home.php
/login.php
/uploads
/admin
/plugins
/classes
/temp
/dist/
/inc
/build
```
In the main page of the site there is a sqli injection we got a database called `cts_db` and we can find a table called `users` and the username is `admin` and we get a hashed password so lets try to crack it and its in MD5 `sweetpan........` and now we can log into the admin panel and after looking around a bit I did some google searching and found a exploit and since it wont work for us with out modifying a lot of it I looked into see what it was doing and its just uploading a php  reverse shell so I uploaded one and then went to the login page and got a call back as `www-data` looking in the web folder and found a file called `DBconnection.php` and has some db creds in it the password for the `cyrus` account is the same as the admin account and then there is a anti virus script in the `/opt/scan/` folder we can use this to exploit the script to read the root flag with `sudo /opt/scan/scan.sh` and then just provide the path `/root/root.txt` and it will make a file and we got root flag or we can just go the easy way and use PwnKit 