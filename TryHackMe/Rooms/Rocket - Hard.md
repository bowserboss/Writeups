Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
```
The nmap scan shows a redirect to a `http://rocket.thm` going to start with a gobuster scan
```
/search
/produects
/files
/page
/people
/prodeuct
/pages
/homepage
/bolt

```
The read more button does not work and the signup for products does not work the only thing I can see that works is the search function so I was looking around for some more and ran whatweb and it found it was running `boltcms` and if we go to `/bolt` we get the login page for the cms its the default path and while I was doing some searching around the site I was running a subdomain scan and found one `chat.rocket.thm` after doing some searching could not find the version of rocket chat running so I took a guess and there is a exploit to reset the password for the admin account and then there is also a nosql exploit we can use and now we got a call back as the user `rocketchat`  looking at the `env` file there is a mongo db running on port `8081` and we can transfer chisel to the machine by encoding it will base64 but we cant auth to it but there is a exploit for `mongoDB` `CVE-2019-10758` to get RCE and we can get a reverse shell and we can get the user db and extract some creds and we got a hash for the user `Terrance` and it cracked to `1q2w3e4r5` but when we try to login it don't work on the bolt cms but if we switch to admin user and this password and now we can login and if we go to configuration and then all config files we can change a php file to a reverse shell and then get a call back as the user `alvin` we have cap suid on ruby we can go to GTFObins but we need a real shell for this work so we need to make the `.ssh` folder and use `ssh-keygen` and then make the `authorized_keys` file and set permissions to `700` and then the `.ssh` folder to `700` as well and then we can SSH into the host and use this cap vuln to get root 