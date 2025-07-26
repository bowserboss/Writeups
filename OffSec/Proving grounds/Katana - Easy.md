Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.9p1
80 Apache 2.4.38
7080 LiteSpeed
8088 LightSpeed
8715 nginx 1.14.2
```
The FTP does not have anonymous login so moving to the first webserver its just a picture of a katana so going to run a gobuster scan
```
/ebook
```
Going to `ebook` its `CSE bookstore` and going to the admin login we can just login as the admin account with `admin` and anything as the password 
Going to the webserver on port `7080` and we cant get it to load so going to the next webhost
The webhost on port `8088` loads the same image as the first webhost gonna run a gobuster scan on this one
```
/img
/cgi-bin
/docs
/upload.php
/upload.html
/css
/protected  Login page
/blocked
/phpinfo.php
```
Going to port `8088` there is another login page 
On port `8715` its another login page but we can just login in with `admin:admin` and its another katana going to run nikto on this one 
Going back to the port `8088` since its the most promising with the two upload functions we found testing them we can upload a file and the webserver on port `8715` will render it and we can upload php files so we can use this to get a reverse shell and we get a call back as `www-data` in the `/etc/passwd` file there is two hashes lets see if we can crack them we can not so running linpeas we find the `python2.7` has `cap +ep` set and we can abuse this to get to the root account with `/usr/bin/python2.7 -c 'imprt os; os.setuid(0); os.system("/bin/bash")'` and now we are root user 