Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 8.2p1
53 
```
The FTP has anonymous login in the FTP there is a file called `backup-OpenWrt-2023-07-26.tar` in the config file there is a WIFI password `.....................!` and we can see a passwd file which has usernames in it and the `netadmin` user used the same password there is a tool on the host called `reaver` using the `mon0` interface searching for caps we found reaver has a cap and when we do ifconfig there is a `mon0` interface and a `wlan0` interface so using this command `reaver -i mon0 -b 02:00:00:00:00:00 -vv --no-nack` we can get the password which is the root password `.....................!` and now we can get the root flag 