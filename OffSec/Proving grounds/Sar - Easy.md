This room gives us creds `love:CrazyGoneAway374` 
Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
```
When going to the site its just a default apache page going to run a gobuster scan 
```
/robots.txt
/phpinfo.php
```
Looking at the `robots` file it says `sar2HTML` so seeing if this is a folder on the webhost and it  is and looking this up on google there is a RCE for this program and we can confirm it is running version `3.2.1` and thats the one we are looking `EDB-ID:47204` and we have RCE now as `www-data` and to get a reverse shell we need to use a url encoded payload and then stabilize the shell and now we are `www-data` and the first flag is in the `/home` folder and we have a user named `love` going back to the web folder  there was two scripts in there called `finially.sh` and `write.sh` and we can modify the write sh file and put a reverse shell in it and we get a call back as the root user after a couple of minutes 