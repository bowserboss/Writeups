Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41 Ubuntu
1337 
```
Starting a gobuster on the website
```
/index.php
/misc
/css
/imgs
/install
/db
/app
/js
/api
/upgrade
/javascript
/config.php size 0
/functions
```

In the `/db/` folder in the `SCHEMA.sql` there is a has that in hex that decodes to maybe SHA-512
Starting with the web server looking at robots.txt file the listing goes to youtube

Doing a netcat on the port `1337` it has some questions for us when answered right gives us login creds `admin:OllieUn......` 
There is a RCE exploit for this version of `phpIPAM 1.4.5`  no CVE for the exploit 
Used the RCE to get a shell onto the host and looking at the `config.php` I found more creds says its to the local db
`phpipam_ollie:IamDah1337est......` 
To get a better shell we used `mkfifo` and url encoded it and it looks like they reused passwords we can su into `ollie` with the pass for the admin account for the website I ran linpeas did not see much so I downloaded `pspy64` and seen that root was running a file called `feedme` looking at this file its a bash script with nothing in it so I put a revshell since we can edit the file and now we are root