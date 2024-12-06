Started with a nmap scan
```
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.29 ((Ubuntu))
```
There is a domain linked to the website `http://swagshop.htb/` going to start with a subdomain scan did not find any subdomains and when going to the site its running `Magento` going to also start a gobuster scan 
```
/media                (Status: 301) [Size: 312] [--> http://swagshop.htb/media/]
/index.php            (Status: 200) [Size: 16593]
/includes             (Status: 301) [Size: 315] [--> http://swagshop.htb/includes/]
/lib                  (Status: 301) [Size: 310] [--> http://swagshop.htb/lib/]
/install.php          (Status: 200) [Size: 44]
/app                  (Status: 301) [Size: 310] [--> http://swagshop.htb/app/]
/js                   (Status: 301) [Size: 309] [--> http://swagshop.htb/js/]
/api.php              (Status: 200) [Size: 37]
/shell                (Status: 301) [Size: 312] [--> http://swagshop.htb/shell/]
/skin                 (Status: 301) [Size: 311] [--> http://swagshop.htb/skin/]
/cron.php             (Status: 200) [Size: 0]
/LICENSE.html         (Status: 200) [Size: 10679]
/var                  (Status: 301) [Size: 310] [--> http://swagshop.htb/var/]
/errors               (Status: 301) [Size: 313] [--> http://swagshop.htb/errors/]
/mage                 (Status: 200) [Size: 1319]
```
running `magescan` we find its version `1.9.0.0` there is a exploit for this version called `magento shoplift` and it will give us access to the admin account of the site we can find a script on github and used chatgpt to make it python3 now we can run the script and it makes us a admin account there is also a authenticated RCE for this version as well https://www.exploit-db.com/raw/37811 the exploit needs some modification and gives us the file to look at and we get some creds 
```
<username>root</username>
<password>fMVWh7bDHpgZkyfqQXreTjU9</password>
```
and after we fixed that rce script we can exacute commands on the host now and get a reverse shell as `www-data` if we run `sudo -l` we can edit any file in the `/var/www/html` folder and get root `sudo /usr/bin/vi /var/www/html/php.ini.sample -c ':!/bin/bash'` and now we are root 