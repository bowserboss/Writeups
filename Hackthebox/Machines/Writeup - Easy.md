Started with a nmap scan
```
22 OpenSSH 9.2p1
80 Apache httpd 2.4.25
```
We cannot fuzz the webpage or subdomain because we will get ip banned from the site but in the nmap scan it showed a `robots.txt` file which had one entry in it `/writeup/` when we go to the writeup folder and if we open the source for the page it says its `CMS Made Simple` and googling the source of the site we can see how the folders work and there is a doc folder with a `CHNAGELOG.txt` that gives us the version of the cms `2.2.12` and there is a exploit for SQL injection `CVE-2019-9053` and if we don't put the `/wrietup/` we will get banned 
```
[+] Salt for password found: 5a599ef579066807
[+] Username found: jkr
[+] Email found: jkr@writeup.htb
[+] Password found: .........................
```
and we can use hashcat to crack the hash with mode `20` pass first then salt and it cracked to `.................` and we can use this to login into SSH we can modify the path and make a file in the `/usr/local/bin` called `run-parts` and add this to the file we made and now we can ssh into the host as root
```
!#/bin/bash

touch /tmp/test
mkdir ~/.ssh
echo 'ssh-rsa SSH KEY ' >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```
