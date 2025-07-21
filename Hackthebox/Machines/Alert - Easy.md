Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
12227 unkown
```
The nmap scan shows there is a domain for the website `alert.htb` did a sub domain scan and found only one `statistics` it is a login page 
Started a gobuster scan
```
/index.php
/uploads
/contact.php
/css
/messages
/messages.php
/visualizer.php
```
Looking at the site there is a contact forum that the admin checks I tried xss in that was getting response back but could not do anything with it so I went back to the markdown forum and tried so payloads there and got another call back that was url encoded so now we can try a LFI for the messages page and we got one and can read the `.htpasswd` for the statistics site and it has a login info for the ssh account `albert:manchesterunited` when I ran linpeas I found a webservice running on port `8080` so I forward the port and its a website monitor when I ran ps aux to look at the processes and root is running this site and there is a folder in the site we can add files to `/opt/website-monitor/config` and we put a reverse shell in the folder and go to the website and now we are root  