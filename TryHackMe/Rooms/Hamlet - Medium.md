Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.6p1 Ubuntu
80 lightspeed 1.4.45
501 Nagios NSCA
8000 Apache 2.4.48
8080 WebAnno 
```
Checking out the FTP service first we have 2 files called `passowrd-policy.md` and `ufw.status` the first file is a password policy new passwords should be `lowercase` and `12-14` characters long the other file is a file status 

Starting a gobuster on port `80` the `index` page says Michael is also known as `ghost`
```
/hamlet.txt
/index.html
/robots.txt ## flag 1
```

The webserver on port `8000` is the `hamlet.txt` file 
 
Web service on port `8080` is a `WebAnna` version `3.6.7` the hint says we will probley make a wordlist to test against `wabanno` so I used cewl and made a wordlist from the hamlet txt file and then used grep and awk to sort from `12-14` words be careful when using hydra you can crash the webserver and we got the login for the app now `ghost:............` are account is admin so we can change the password for other users on the app when we access the `ophelia` account and look at the hamlet.txt file there is a note with a password `............` and we can access the FTP with this account and get the 3rd flag and there is a python file also that has the second flag so now were at another dead end going back to trying to get a reverse shell on the web app so when we go to the hamlet project we can upload any files we want and then get to them on the webhost on port `8000` after we upload the shell we go to this link and get a call back `/repository/project/0/document/1/source/php.php` and now we are `www-data` but now we are in a docker container but we can use `cat` as root and read files so we can read shadow and passwd file and crack the hash with john and now we have the root password `murder` and we can find flags 4 and 5 and now its time to break out there are 3 breakouts you can do once you got one done we can read the root flag I chose mounting breakout 

Possible usernames
```
Michael
ghost
admin
ophelia
ubunutu
gravediggers
```
