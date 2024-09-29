Started with a nmap scan
```
22 OpenSSH 8.2p1 Ubuntu 
80 Apache 2.4.41
```
And the nmap scan gave us a domain as well `http://olympus.thm` going to the site says its still under development and says the old site is still accessible on this domain did a subdomain fuzz and did not find anything so now I'm going to gobuster the site 
```
/index.php
/static
/javascript
/phpmyadmin
/~webmaster/
```
Going to the webmaster folder its running `Victor CMS` testing out some of the inputs on the site the search has a sql injection vulnerability and looking on google there is one exploit-db  dumping the database `olympus` there is a flag and some users got the hashes trying to crack them
```
prometheus:sum.........
```
And now we can log into victor cms when we dumped the database in the emails there is a subdomain `chat.olympus.thm` and when we go to this we get another login page and we can use the same login from the cms on this site and we go back to the database there was a section of chats and we can get the file names and go to the `uploads` folder and we can see the file now when can upload a reverse shell and dump the database agin and get the file name and then we can go to it and get a call  back as `www-data` running linpeas we see we can run `cpuutils` and with this we can copy files so we go to `zeus` home folder and run the cputils and do this `./.ssh/id_rsa` and then name the file and now we have the ssh key for the user `zeus` the ssh file is password protected so we use `ssh2john` and crack it with john `snow.....` is the password now that we got shell access as zeus there was a folder in the `www` area that was owned by the zeus group and there is a php file thats a root backdoor and there is a binary in that php code we can use to get root `uname -a; w; /lib/defended/libc.so.99` and we are now root  