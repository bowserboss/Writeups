Started with a nmap scan
```
22 OpenSSH 8.2p1 Ubuntu
80 Apache 2.4.41 
```
Nmap scan shows there is a domain linked to the website `http://lookup.thm` 
Going to start a subdomain scan did not find any
Going to start a gobuster scan
```
/index.php
/login.php
```
So when going to the website there is a login page tried some sqli did not find anything but when I was testing random usernames I noted the error messages would change so we can try to automate this with ffuf or a python script found two usernames `admin,jose` trying to crack both users with hydra and the user `jose` cracked `password123` and when we log in we get a new subdomain `files` its a file host called `elfinder` version `2.1.47` and when we look for exploit for this version there is a command injection vulnerability `CVE-2019-9194` now we have a shell as `www-data` and running linpeas there is a unknown binary called `pwn` and when running strings on the program we can see we can change are path and its looking for a file called `.passwords` which the user `think` has so we change are path to tmp an make a id file that echo's out think's uid and gid 
```
echo '#!/bin/bash'
echo "echo 'uid=33(think) gid=33(think) groups=33(think)'"
```
and then running the program gives us what looks like a list of passwords so can run use this and run hydra running hydra we get the password `josemario.AKA(think)` and now we can ssh into the host when we run `sudo -l` we can run `/usr/bin/look` if we go to gtfoBins there is a sudo exploit for it 