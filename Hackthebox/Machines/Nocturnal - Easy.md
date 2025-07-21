Started with a nmap scan
```
22 OpenSSH 8.2p1
80 nginx 1.18.0
```
The nmap scan shows that the webserver has a domain that it redirects to `http://nocturnal.htb` checking for subdomains and did not find any going to start a feroxbuster scan
```
/uploads
/login.php
/register.php
/backups
```
Going to the register we can make a account and now login and we have a file upload forum we can only upload these file types 
```
pdf
doc
docx
xls
xlsx
odt
```
Looking at the link to upload files it has the username in it and we can fuzz the usernames to see what users are on the host 
```
admin
amanda
tobias
```
and if we view `amanda` files we see a `privacy.odt` file running `file privacy.odt` and its a zip file we can unzip it to help read the metadata reading the `content.xml` has a username of Amanda and also has a passowrd `arHkG7HAI68X8s1J` and we can use this login for the site and we get access to the admin panel and we can make a backup of the site and download it looking at the admin pannel there is a way to upload files without it being in the uploads folder and can get command injection on the `amdin.php` file in the password field and make a cmd shell so we can exacute commands as `www-data` and download a revshell script and run it now were on the host and looking around the web folder we found a DB so now we can get it onto are host and we cracked 1 hash the user `tobias` password `slowmotionapocalypse` and we can SSH as this user now time to run linpeas and there is a webserver running on port `8080` so we need to forward the port with SSH and now we can access the site and its `ISPconfig` and its running version `3.2` and testing logins from google we know the user is probley admin and the password for SSH worked as the admin password and its vulnerable to `CVE-2023-46818` and we run the exploit poc from github and now we are root 