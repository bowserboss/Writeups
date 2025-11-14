Started with a nmap scan
```
PORT   STATE    SERVICE REASON              VERSION
22/tcp filtered ssh     port-unreach ttl 61
80/tcp open     http    syn-ack ttl 61      Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Example.com - Staff Details - Welcome
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
```
Going to the website we have a web page with a login and a search function testing the search we have a sqli injection in it to UNION query and some others tested with sqlmap and we have two databases `staff` and `users` in the staff database there is a admin user with a hash we can crack it to `transorbital1` and in the users database there is a users and password columns and we can login with the admin user and there is a error on the footer of the page we can fuzz the manage page for parameters  and we find `file=`  and its vulnerable to LFI we can use this to take a guess and read the `knocked` file since SSH is filtered and we got a away to open up SSH `7469,8475,9842` and use knock to knock on the ports and now SSH is open so using are username and password list we cracked 3 users for the SSH 
```
chandlerb:UrAG0D!
joeyt:Passw0rd
janitor:Ilovepeepee
```
using the `janitor` login there is a folder called `.secrets-for-putin` and there is a password list we can try and cracking more logins with this and we cracked another login `fredf:B4-Tru3-001` and this user has the first flag and this user has sudo permissions on `/opt/devstuff/dist/test/test` if we run this we can just do `sudo /opt/devstuff/dist/test/test '/etc/shadow' /tmp/test` and we get back the shadow file so we can just do `sudo /opt/devstuff/dist/test/test '/root/proof.txt' /tmp/test` and we have the root flag 