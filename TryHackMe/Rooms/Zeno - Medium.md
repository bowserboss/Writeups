Started with  a nmap scan
```
22 OpenSSH 7.4
12340 Apache 2.4.6 CentOS PHP 5.4.16
```
Started a gobuster scan on the webhost 
```
/index.html
/rms
/rms/images
/rms/index.php
/rms/contactis.php
/rms/aboutus.php
/rms/galley.php
/rms/admin
/rms/footer.php
/rms/cart.php
/rms/css
/rms/logout.php
/rms/ratings.php
/rms/auth.php
/rms/fonts
/rms/tables.php
/rms/swf
/rms/inbox.php
/rms/connection
/rms/stylesheets
/rms/validtion
/rms/specialdeals.php
```
Tested the login page for sql injection I did find a RCE for this `Restaurant Management System 1.0` and now we got a shell on the server to get a reverse shell I went to `revshells.com` and used python #2 and we are now the apache user in the webserver there is a connection folder that has a user and password to the mysql DB `root:veerUffI......` in the `dbrms` database there is 2 members that we can crack the login hash for there stored in `MD5` and the answer for the security questions there is a secret share mounted in the `fstab` with creds for a user called `zeno` with a password and its also the password for the user `edward:Frobj.............` and now we have SSH access to the server after looking around for a bit in this user I did not find much as a way to get to root but we can get root with pwnkit exploit 
  