Started with a nmap scan
```
22 OpenSSH 7.4
80 Apache 2.4.6 Centos PHP 5.4.16 Drupal 7
```
Looking at the site we cant do much we cant make a account I did not see any sqli in the login page but the site is running `Drupal 7` and there is a exploit for this on metasploit `CVE-2018-7600` and we got a shell as `apache` looking in the config files we find the login for the database to use the database we need to use the `-e` options to run commands and when we dump the users table and get a hash for the user that's on the machine and it cracked `brucetherealadmin:booboo` and when we run `sudo -l` we can run `snap install *` as root so we have to make a snap package and then install it and we can get root 