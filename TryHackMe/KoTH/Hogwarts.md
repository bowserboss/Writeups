Started with a nmap scan
```
22 Apache 2.4.38
8667 PHP cli server 5.5 or less
9213 OpenSSH 7.2p2 Ubuntu
8835 vsftpd 3.0.3
55494 ? webserver telnet this
```
To access this website we need to curl it but we can run a gobuster scan
```
/240
/inex.html
/uploads
/robots.txt
/strona_3
/rw1v6iftum
/hint.txt
```
On note `240` says the safest port to have a upload server is on `22` and has a word that's base64 encoded `bWY5aXZ3bzhndW==` and its a page on the webhost 

The FTP has anonymous login on it and there is a note that says it might be a dead end 

Going to the next webserver `8667` its a login page and when I enter `'` we get a SQL error in are syntax so we got sqli injection most likely and we do have a injection we can do dumping the students database and we get a table called `non_magic` we got a ssh login `hermoine:ea@58mbym9mq683c1w0#g7c#3` now that we are on the host when we run `sudo -l` we have root access to run `/bin/date` if we go to GTFObins we can exploits this to read files I got the shadow file and the passwd file and Im trying to crack the hashes could not crack the hashes with rockyou.txt but we can use PwnKit to get root to account



Possible usernames
```
Hagrid
Dumbledore
```

Flags
	1. flag in the mysql database in the basement
	2. hermoine user folder  
	3. /etc/left_corridor/seventh/.entrance
	4. /root/headmaster.txt
	5. /home/drace/achievements.txt
	6. /home/harry/special_spell.txt