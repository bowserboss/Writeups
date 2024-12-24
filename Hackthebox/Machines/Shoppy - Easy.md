Started with a nmap scan
```
22 OpenSSH 8.4p1
80 nginx 1.23.1
9093 copycat?
```
Looking at port `9093` its a heap dump of something so going to look at the webserver and running a  gobuster scan
```
/images
/login
/admin
/assets
/css
/js
/fonts
/exports
```
and when we go to the website its a page saying its coming soon there is also a domain linked to the site `http://shoppy.htb/` the admin and the login page seem the same and both when you try to log in and basic creds dont work  going to do a subdomain scan and see if we can find anything we find one subdomain `mattermost` going back the login page we can try a noSQL auth bypass after testing login bypass `username=admin' || 'a'=='a&password=admin` and now we can get access to the matter most panel if we run the same payload for the username and change the a to a 1 it gives us a download page and we get hash for another user `josh` and we can crack this hash `remembermethisway` now with this login we can auth to the matermost page as josh looking in the chats we can find the ssh login for the user `jaeger` when we run `sudo -l` there is a password manager binary we can run as deploy when we run strings on it to maybe get a idea of what it is did not find much so I got the binary to my system and run ghidra we can find the password for the binary `Sample` and now we have creds for the deploy user and we can run the docker image as a privilged user `docker run --rm -it -v /:/mnt chroot /mnt sh` and now we are root