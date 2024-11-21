Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
139 smb
445 smb
3000 http?
5000 Node.js
```
Starting with the smb server going to enumerate this first while running gobuster scans there is one share we can access with out a login `traces` 
All shares 
```
traces
print$ Denied
IPC$ Denied 
```
Users found these users don't need a password for smb
```
moana
maui
network
```
Gobuster scan for port `80`
```
/index.html
/javascript
/hidden.html page says 'hope you like ziplines and rabbits too'
```
On port `3000` found nothing right now but the api runs on this port 
time to scan the domain we found
```
/index.php
/docs
/docs/ROUTES.md
/javascript
/v2/login
/v2/jobs
```
In the smb there is a bunch of pcap files there all empty but the one for `maui` it has some http request in it to a `dashboard.png` we can get the png file from the pcap file and when I open it gives us a domain that says its for the developers of Motunui which is also the hostname for this machine `d3v3lopm3nt.motunui.thm` says we need authorization to view it we need to find a way to access the page I did a subdomain scan and did not find anymore subdomains looking the in docs folder I found a `readme.md` file that told us to go to `routes.md` and we get the api that's on port `3000` also when I was looking at the pcap file it had a website in it and the site links to a tool called `JSONbrute` which we can get from his github and use it to brute the login api we found took me a little bit to find a username but when I downgrade the api to v1 we get another potential username since the other one did not work `maui` got the password now `island` now if we send a curl command we can get the hash
`curl http://api.motunui.thm:3000/v2/login -H "Content-type: application/json" -d '{"username":"maui","password":"......"}'` and now we have the hash so we can get to the other endpoints now to auth with the hash we got `curl http://api.motunui.thm:3000/v2/jobs -H "Content-type: application/json" -d '{"hash":"......."} -X GET'` then to make a cronjob `curl http://api.motunui.thm:3000/v2/jobs -H "Content-type: application/json" -d '{"hash":"aXN...sYW5k","job":"* * * * * revshell"} -X POST ` and now if we get the request again it will show in the cron jobs and we get a call back as `www-data` there is a `pkt` file in the `/etc` folder and we have to get CISO packet tracer and open the switch and export the config we get the creds for `moana` for SSH access `.......'LLG0` looking back at the web stuff the tls-html is running as root but we can edit the server.js file as `www-data` so we can make it chmod `/bin/bash` and then crash the program with a tool called goldeneye and that will cause it to restart and exacute are code  and we will have root or we can do it the easy way and there is a `ssl.txt` we can import into wireshark and get the root login 