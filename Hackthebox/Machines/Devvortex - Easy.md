Started with a nmap scan
```
22 OpenSSH 8.2p1
80 nginx 1.18.0
```
There is a domain linked to the webhost `devvortex.htb` doing a subdomain scan found one subdomain `dev` doing a gobuster scan
```
/index.php
/images
/home
/media
/templates
/modules
/.git/
/plugins
/readme.txt
/components
/api
/cache
/libraries
/robots.txt
/LICENSE.txt
/layouts
/administrator
```
The dev site is running `Joomla CMS` version `4.2.6` and its vulnerable to `CVE-2023-23752` using the metasploit exploit for  it we got the user and password now to the cms `lewis:................##` at the `/administrator` link on the dev site so what we can do for easy reverse shell from the site is go to the admin section and update the error page of the template with a normal php reverse shell and we get a call back as `www-data` now we need to get to the user `logan` looking at the `config.php` file it shows us the login for the mysql database and we can login in with the user `lewis` with the same password as the `joomla` panel now we have a hash for the user `logan` `$2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGT............` and we cracked it to `.................` and now we can ssh into lewis account and get a real shell when we do `sudo -l` we can use the `/usr/bin/apport-cli` binary and root nad it has a `CVE-2023-1326` so now we can get root running `ps -ux` and we get the PID for system and then we can run this command `sudo /usr/bin/apport-cli -f -P 18046` and then hit v and then `!/bin/bash` 