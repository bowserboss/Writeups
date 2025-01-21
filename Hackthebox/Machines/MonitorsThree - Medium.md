Started with a nmap scan
```
22 OpenSSH 8.9p1
80 nginx 1.18.0
```
There is a domain is the website `http://monitorsthree.htb` 
Starting a gobuster scan on the website
```
/images
/index.php
/login.php
/admin
/css
/js
/forgot_password.php
/fonts
```
Doing a subdomain scan and only found one `cacti` it is vulnerable to `CVE-2024-25641` but we need creds first after testing sql injections on the main site I found one in the password reset a time based blind injection got a list of 4 users
```
admin
dthompson
janderson
mwatson
```
I after dumping all the password as well I cracked only one of them and its for admin `greencacti2001` and it logs in on both sites and now we can use that exploit for the cacti program and get a shell as `www-data` and looking in the web folder I found a `config.php` in the includes folder of cacti and got some creds for the mysql server `cactiuser` for both username and password and then we got some more hashes to crack cracked 2 hashes to `12345678910` and its to the user `marcus` on the server there is a service running on port `8200` and we need to forward the port to us so we can see the webapp for `duplicati` and I was looking in the file for duplicati and found what I think is a password with a salt but first we need to get the web app to us now looking at the webapp we get a login page time to look back at the hashed creds we might of found in the database in the duplicati folder and we need to forward the port `8200` there is a login bypass we can do its issues `5197` on the github for duplicati when doing this bypass just have to make sure to replace the passowrd before we forward the request and now we have access to the dashboard and we can make a backup of the root flag and restore it were we want on the host and now we have root flag