Starting with a nmap scan
```
22 OpenSSH 8.2p1 Ubuntu
80 Apache 2.4.41
```

Starting a gobuster scan with `2.3-medmium` and also with the php flag
```
/images
/index.php
/about.php
/contact.php
/css
/do.php
/js
/portfolio.php maybe?
```

Ran a nikto scan found nothing
Running some nuclei templates I have
While I was running scans I was manually poking at the site and noticed that there not much going on with the main site and found this domain `board.htb` 
Did a subdomain fuzz and found one domain `crm.board.htb` this goes to a login page for `Dolilbarr` version `17.0.0` found a CVE for it `CVE-2023-30253` and now we have user as `www-data` in the home folder there is a user called `larissa` 
I found a login for a MySQL db on port `3306` `dolibarrower:serve..........` db name `dolibarr`
I looking in the db found nothing then I tried the db password to login into `larissa` and now we got user access now to get root access we ran linpeas and got some interesting stuff saw that it has a LPE vuln `CVE-2022-37706` that using the enlightenment binary and this will give you root