Started with a nmap scan
```
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux
53/tcp open  domain  syn-ack ttl 63 ISC BIND 9.9.5-3ubuntu0.14 (Ubuntu Linux)
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.7 ((Ubuntu))
```
Starting with a gobuster scan since the webserver is just a default apache page 
```
/index.html
```
Did not find anything so im doing to add a domain to my hosts file of `bank.htb` and see if it has any subdomains or  we can run this command `dig axfr bank.htb @10.10.10.29` and find the sub domain `chris.bank.htb` but it goes back to a apache web page but now the main site since I added the domain is now  a long page 
now going to re run the gobuster scan
```
/index.php            (Status: 302) [Size: 7322] [--> login.php]
/login.php            (Status: 200) [Size: 1974]
/support.php          (Status: 302) [Size: 3291] [--> login.php]
/uploads              (Status: 301) [Size: 305] [--> http://bank.htb/uploads/]
/assets               (Status: 301) [Size: 304] [--> http://bank.htb/assets/]
/logout.php           (Status: 302) [Size: 0] [--> index.php]
/inc                  (Status: 301) [Size: 301] [--> http://bank.htb/inc/]
/balance-transfer     (Status: 301) [Size: 314] [--> http://bank.htb/balance-transfer/]
```
if we go to the `balance-transfer` we see a lot of files ending in `.acc` and if we open one up it shows us encrypted information about the account there is one file that has a size of `257` and its not encrypted and has a login of 
```
Email: chris@bank.htb
Password: !##HTBB4nkP4ssw0rd!##
```
and we can log into this account if we look at the `support.php` there is a commit for debugging which says there is a file extension `.htb` and this will exacute as php so we can try and php reverse shell on the upload now we have a shell as `www-data` user running linpeas shows us there is a unknown suid file `/var/htb/bin/emergency` if we just run the file we are root 