Started with a nmap scan
```
22 OpenSSH 7.2p2
80 Apache 2.4.18
```
Started with the webapp doing a gobuster 
```
/index.php
/shop.php
/assets
/item.php
/roobots.txt
/delete
/requests.txt
```
After looking at some of these web pages there is a Sql Injection in the item page and we got some databases now we can look at there is a user table in the database and we can use the password we found and ssh into `dennis` password `ih8.....` the first flag is in the home folder the 3rd flag is in the `.bash_history` file could not find flag 2 at the time but I found a path to root we can run sudo with `scp` so we can go to GTFObins to exploit it and now we are root I ran linpeas as root and found the second flag in the `/boot/grub/fonts/flagTwo.txt` 