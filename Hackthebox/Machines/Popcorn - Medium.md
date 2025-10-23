Started with a nmap scan
```
22 OpenSSH 5.1p1
80 Apache 2.2.12
```
The nmap scan shows there is a domain `http://popcorn.htb` going to the site its a apache page that says it works so running a gobuster scan
```
/test
/torrent
/rename
/torrent/uploads/
```
The `/test` is a `phpinfo` page the `/torrrent/` is a site and the `/rename` shows a API path
Checking out the web site first there is a login page it is vulnerable to sql injection but we cant crack the hash for the admin account so we can make a account and there is a torrent upload we can make a fake torrent and then upload it and we can change the picture for the torrent and capture the request when uploading the picture and add some php code for a reverse shell and then we can go to `/torrent/uploads/` and the php file will be in there and we get a call back as `www-data` looking in the web folder for torrent there is a database folder with a php file  in it and it has a different hash for the admin user `admin:admin12` running linpeas we can see its vulnerable to a couple of exploits that are chained together linux kernel `2.6.37` name of the exploit `full-nelson.c` and now we can get root user 