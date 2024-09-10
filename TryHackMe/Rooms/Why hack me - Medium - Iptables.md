Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 8.2p1
80 Apache 2.4.41
41312 ?
```
starting with the ftp since it has anonymous login enabled there is one txt file called `update.txt` the file says that the user mike because the account was compromised  and creds for the new account are at `127.0.0.1/dict/pass.txt` but can only be access by local host 
Starting a gobuster scan
```
/blog.php
/login.php
/register.php
index.php
/dir
/dir/pass.txt
/assets
/logout.php
/config.php
```
We can make a account and post in the comments I tested the comment section for sql injection did not find anything the login section also is not vulnerable 
The username section is vulnerable though to XSS injection and we can inject a js file to exfill data back to us you can find it on github and we can get the `/dir/pass.txt` and I get a request back in base64 and decode it and we got creds for user `jack:WhyIsMyPasswor.......` the login for the MYSQL server but there not much on it `root:MysqlPasswordIs........` and in the `/opt` folder there is a txt file called `urgent.txt` and says there trying to remove some file but cant and there is also a pcap file but its all TLS encrypted but we can run iptables as root and in the tables port `41312` was set to drop all traffic so we can enable it to accept traffic agin 
`sudo /usr/sbin/iptables -I INPUT -p tcp --dport 41312 -j ACCEPT`
and the reason we cant delete the files because another user owns them going to the new site we can only access with `HTTPS` but we get a forbidden error but maybe since we have access to the server we can get the apache key to decrypt the traffic in the pcap file and we can sort all the traffic by HTTP and we see a link to a shell using the shell we can see busybox is on the server and we can use that to get a revshell as `www-data` with the group of `hacked` and running `sudo -l` and we can run anything as root 