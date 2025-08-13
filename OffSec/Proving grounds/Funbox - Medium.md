Started with a nmap scan
```
21 ProFTPD
22 OpenSSH 8.2p1
80 Apache 2.4.41
33060 mysqlx 
```
The nmap scan shows there is a domain `http://funbox.fritz.box` and there is a robots.txt file that has a folder disallowed called `/secret/` when going to this folder it says there is nothing here also looking around the site its running on `wordpress version 5.4.2` we got 2 user names `admin,joe` could not find any plugins with wpscan now lets see if any of there logins crack and joe login cracks to `joe:12345` and we can login into the site and he has no admin permissions but after a little bit we also got the admin account `admin:iubure` but lets try SSH and we also got access so now we have a shell but it drops us into rbash which sucks so we need to break out of this first so if we do 
```
vi
:set shell=/bin/bash
:shell
```
and now were in a bash shell running linpeas there is a lot of ways to root I just use PwnKit which might not be the intended way there is also a script that maybe root runs as funny that backups the `/var/www/html` folder  