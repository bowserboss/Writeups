Started with a namp scan
```
21 vsftpd 3.0.3
80 Apache 2.4.29
1337 OpenSSH 7.6p1
```
Doing a gobuster scan 
```
/webmasters
/webmasters/admin
/webmasters/admin/index.html
/webmasters/admin/login.html
/webmasters/admin/admin.html  #login goes no were
/webmasters/backups
```
After doing long enumeration on the webhost I found the file in the `backups` folder using common 3 letter extensions it is password protected and has a note we used john to crack the password with rockyou and the pass is `003..07` and we can see the note and got the FTP username `ftpuser`  but no password after using hydra to crack the ftp account we got the password `love..` after getting on the ftp user there a lot of folders going threw a lot of them I found a `id_rsa` and a `not.txt` and it refers to us as james so maybe are ssh username and going to use john to crack the `id_rsa` and got the password `blu...e` when we log in there is a bash script running as root that kicks us out so we have to break out of the shell there is a page on hacktricks and now we can `ls -la condor` and there 2 base64 strings to give us the path to the picture and the pic has something encoded into it called Mnemonic on github and we run the script but it breaks and a little bit or research and found we need to up the max size on the script to `6000` and now we can decode it and the password for `condor` is  `pasific.....1` now we have shell access to `condor` now if we run `sudo -l` we can see we can run a script as root we run it select option `0` and when its typing out do `/bin/bash` and we got root