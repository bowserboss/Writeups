Started with a nmap scan
```
21 vsftpd 2.0.8  ## found this after I was added to the firewall
22 OpenSSH 7.9p1
80 apache 2.4.38 
443 Apache 2.4.38
1025 NFS? ## found this after I was added to the firewall
5432 postgresql DB 9.6.7 - 9.6.12 ## found this after I was added to the firewall
```
The nmap scan shows a domain `https://intra.redcross.htb/` by it being HTTPS on port `80` means its redirecting to `443` 
Going to the site its a `RedCross Messaging Intranet` its a login page tested sqli did not get anything there is a contact page its says `contact staff to request your access creds` tested some xss and there is a blacklist of some sort so going to keep testing going to run gobuster scan while testing this forum
```
/imaes/
/pages/
/documentaion/
/documentaion/account-signup.pdf
/javascript/
/javascript/jquery/
```
we have found a `pdf` file this file is telling us how to get a account in the title `credentials` and in the body we need to `username=bowser` doing this they gave us a guest account `guest:guest` and we have a message from the admin saying they gave us this account and that beta is in beta and there is also a `userID` search option and its vulnerable to sqli `error-based` and `boolean-based blind` and we have a database `redcross` and there is a users table we can dump the hashes of 4 users but the `penelope` user has no hash and `tricia.wanderloo` has no username but we can try cracking the hashes and we cracked one `hashcat hash-sqli rockyou.txt -m3200` and we got the login to `charles:cookiemonster` we can now login and we got 3 messages all talking about a admin panel time to do a subdomain scan `ffuf -u 'https://redcross.htb' -w bitquark -H "Host:FUZZ.redcross.htb" -fc 301` 
```
intra
admin
```
the login we have don't work on the `amdin` site but the session cookie does and there us a user console and a network firewall and we can add are self to bypass the firewall took me a little bit to do this part but I re ran the nmap scan and got two new ports looking around at the ftp we cant auth to it now looking at port `1025` its a smtp server if we do `nc redcross.htb 1025` and then type `HELO redcross.htb` we get `Haraka is at your service` googling this we get `Haraka SMTP` and its running version `2.8.8` we can use telnet to get this info `telnet 10.10.10.113 1025` and do `HELO redcross.htb` 
there is a RCE for this but I could not get it to work so looking back at the admin site the firewall fuzzing the action I found `deny` and trying the command injection agin in the ip section we got RCE this way `ip=10.10.16.4;id` gets me `www-data` if we use a php reverse shell and url encode it and get a call back as `www-data` now looking at the webapp there is a `users.php` which has DB creds in it `unixnss:fios@ew023xnw` I also found the creds to the infra DB `dbcross:LOSPxnme4f5pH5wp` now we can try and auth to the `postgrerss` with `psql -h 127.0.0.1 -U unixnss -d unix` and then enter the password but we cant seem to do anything with this account so we need to enumerate more I found two more sets of DB creds but one has a username of `unixusrmgr:dheu%7wjx8B&` and with this user we can add users to the database with `insert into password_table (username ,passwd, gid, homedir) values ('fakesu', 'hash',27, '/home/penelope/');` now we can SSH with this user and then run sudo bash and now we are full root