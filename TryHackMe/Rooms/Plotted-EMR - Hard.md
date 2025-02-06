Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 8.2p1
80 Apache 2.4.41 (Ubuntu)
5900 MariaDB 5.5.5-10.3.31
8890 Apache 2.4.41
``` 
Testing the FTP there is anonymous login allowed there is two hidden folders `.-` and `...` and then there is a file called `you_are_determind.txt` it was a rabbit hole but gives a hint also `see if we can access the admin account` now lets check out the webserver on port `80` and when we go to its a default apache page so lets start a gobuster scan to see what's on the host 
```
/admin #base64 says this might be a username
/shadow #rick rull
/passwd #rick roll
```
Now time to scan the other webhost 
```
/portal
/portal/admin.php
/80
```
Going to the `/portal` we get a login page for `openEMR` and did a bunch of testing like bruteforceing the login page with the user we got hinted to and tried bruteforceing the mysql service and got blocked but in fact we we can just login with no password to the mysql service 
`mysql ip -u admin -P 5900` but there is nothing useful here so we still need to find the version of this that's running and if we go to `/portal/admin.php` we get the version `5.0.1(3)` there is a metasploit module that does a sql injection and we get some users but non of them worked and I noticed this database was different then the other one so going back to the `/admin.php` page there was something interesting there we could make a new site id of `admin` we need to set the mysql creds we got to this new setup and set the user as anything and then run the setup and then go to the new site and now we can login and from the googling we done eailer there was some RCE exploits the one I'm using is on exploit db `45161` and we have to edit the url line to the new site and then we can get a reverse shell as the user `www-data` now we can run linpeas on the host if we look at the cron's running there is one for `plot_admin` 
`cd /var/www/html/portal/config && rsync -t * plot_admin@127.0.0.1:~/backup` 
so we can make a reverse shell in the config folder and it will get run by the user or we can copy `/bin/bash` and make it a executable with the script and then exacute it on are own 
```
touch shell.sh
vi shell.sh
cp /bin/bash /dev/shm/bash
cp ~/.ssh/* /dev/shm
chmod x+s /dev/shm/bash
chmod a+rwx /dev/shm/*
touch -- '-e sh shell/sh'
```
Then wait for the cron job to run and now we can just get the `id_rsa` key and ssh into the host but i was still missing the first flag so I grep the whole system and found it in the web folder in a file called `ThisFileisnotintersting` running linpeas agin I found we have cap vulnerability over perl and we can abuse this 
```
perl -e 'chmod 0777, "/etc/passwd"'
openssl passwd -1 -salt user pass
echo 'user:hash:0:0:/root:/bin/bash' >> /etc/passwd
su user
```
and now we can get the root flag 