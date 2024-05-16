Started with a nmap scan
```
22 OpenSSH 7.6p1
80 ngnix 1.14.0
```
Starting with a gobuster scan with list `2.3-medmium` 
```
/index
/contact
/products
/login
/admin
/satic   403 forbidden
```
Started a nikto scan also it did not find anything


The login page does not seem to do anything its sending GET request and not POST
Test all the forms with sqlmap found a sqli in the product page with `#1*` and its timed based sqli injection blind
The name of the database is `ducky...` there are two interesting tables `system_user` and `user` the `system_user`  has three logins with hashes 
Found a hash that's bcrypt so we cracked it with hashcat and cracked to `<Redcated>` 
So the user `system_user:<Redacted>` can log into ssh and when I was looking at the `app.py` file in the `/var/www/duckyinc` folder I found the MySQL login `root:<Redacted>` and from the localhost we can login in and dumping the user tables we found flag1 and flag2 is in the server-admin home folder 
For the 3rd flag we have to deface the website just edit in the index.html file and then  the flag will show in the root folder