Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
161 UDP smtp
```
Going to the website its a pretty static site all we have is a contact forum ran a gobuster on the site there is nothing and were at a dead in gonna re run nmap scan with udp and see if we get anything and we got a new port `161` and also ran `sudo nmap -sU 10.10.11.136 -sCV -p161` and this gives us the creds for a user `daniel:HotelBabylon23` and we can SSH into the host now looking around in the `/var/www/pandora_console` there is a new webapp running on port 80 and we can port forward so we can view it on are localhost `ssh daniel@10.10.11.136 -L 1234:localhost:80` and this site is running `pandora FMS` and its running version `7.0NG.742_fix_perl2020` there is a SQL injection we can exploit and once we get creds there is a authenticated RCE the sqli is `CVE-2021-32099` the database is `pandora` when I was dumping the DB I was messing around with the site and it loged me in as admin and I could change passwords and stuff now we can use the next exploit but we have to do this manual `CVE-2020-13851` and edit the `ajax.php` POST request and then use a python reverse shell now we have access as the user `matt` and can get the user flag gonna run linpeas now and we found not a normal binary `/usr/bin/pandora_backup` has SUID bit set getting the binary onto are host and running strings on it we can see `tar` is not using a absolute path so we can do a PATH variable hijack 
```
cd /tmp
export PATH=/tmp:$PATH
echo -ne '#!/bin/bash\ncp /bin/bash /tmp/bash\nchmod 4755 /tmp/bash' > tar
chmod +x tar
```
and then run the backup program and now we are root 