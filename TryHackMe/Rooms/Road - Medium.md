Starting with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
```
Started a nikto scan it found the phpMyAdmin

Started a gobuster scan with list `2.3-medmium` 
```
/assets
/v2
/v2/ResetUser.php
/v2/profile.php
/index.html
/v2/admin/login.html
/career.html
/phpMyAdmin/
```
Tested the login for with sqlmap and ghauri found nothing
Made a account we can login but the only thing that works after you login is the order tracking and password reset the order tracking says they had to disable it the order tracking sends only a GET request
The server has phpMyAdmin running version `5.1.0` 
When poking around the site some more and looking at the password reset page I sent the request to burpsuite and changed my email with `admin@sky.thm` and changed the password and now we can login as admin when we had just a user and you go to the profile page it said only admin can upload profile pictures so now were admin lets have a look at that and it takes any file extension tested with just a `sh`  script and it uploaded under the `/v2/profileimages/` and we can download the sh script so if we upload a php reverse shell we can get the code to exacute and get a call back to are host now we are `www-data` and we can read the user flag and using PwnKit we were able to get root
