Started with a nmap scan
```
22 OpenSSH 7.2p2
80 Apache 2.4.18
```
There is a domain linked to the web app `jack.thm`  and its running wordpress core version `5.3.2` 
Ran wpscan found 3 usernames 
```
jack
wendy
danny
```
The theme in use is `online-portfolio` I tried to enumerate all plugins but did not find any 
Starting a gobuster scan with `2.3-medmium` 
```
/index.php
/login
/wp-content
/wp-login.php
/license.txt
/javascript
/readme.txt
/robots.txt
```
Time do brute force the web login its the other thing we have left I used a couple different wordlist and then I used the `fasttrack.txt` and got a hit on username `wendy` with the password of `changelater` after looking around a bit I found nothing but I did find a plugin called `user-role-editor` and it has a exploit to make us admin if we update are profile and add this to the request `&ure_other_roles=administrator` and now we are admin and to get a reverse shell we upload a plugin with a php shell in it and now we are on the server as `www-data` running linpeas on the host we found a `id_rsa` key in the `/var/backups` and its the key for jack so now we have a real shell on the host also when I ran linpeas I noticed that jack can edit `python2.7` files and found this `checker.py` file in the `/opt` folder that checks to see if the site is up so I ran pspy to see if its on a cron job and it is as root so I checked the python file and it only imports the os so I edit the os.py file and put a reverse shell in the file and waited for the cronjob and we are now root 