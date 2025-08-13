Started with a nmap scan
```
22 OpenSSH 7.9p1
80 Apache 2.4.38
```
Nmap shows the web page tittle and its `monitorr` if we google this there is a un auth RCE for this version `1.7m` and we can use this to get a shell as `www-data`  running linpeas there not much with this host other then a couple exploits we can do to get to root like PwnKit 