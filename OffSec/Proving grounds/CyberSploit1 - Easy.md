Started with a nmap scan
```
22 OpenSSH 5.9p1
80 Apache 2.2.22
```
Going to the webserver its just a static website with nothing to click on going to run a gobuster scan
```
/index
/hacker ## Picture
/robots  ##base64 youtube channel
``` 
Not finding much looking back at the source code there is a username in the index page `itsskv` and the password is `cybersploit{youtube.com/c/cybersploit}` and now we can ssh into the host running a `uname -a` its using kernel version `3.13.0-32` and this is vulnerable to the `overlayfs` exploit `CVE-2015-1328` and we are now root user 