Started with a nmap scan
```
22 OpenSSH 7.9p1 
80 nginx 1.14.2
8065 unknown
```
If we go to port `8065` and looking around there is a domain linked to the webhost `delivery.htb` doing a subdomain scan now if we go to the helpdesk we can make a ticket and it gives us a email we can use this email to verify a account on the matter most domain and verify are account and now we can access the internal chat on the matter most and this gives us creds to SSH into the host `maildeliverer:..................` after searching for a while I found were the mysql password is `/opt/mattermost/config.json` `mmuser:......................` and we get some hashes from the users table and one says root we can try cracking some of these if we look back at the internal chat it says something about rules to I tried the `best64.rule` and said something about `PleaseSubscride!` is not in rockyou and cracked the password `hashcat -m 3200 hash -r best64.rule wordlist` and it cracked `................!21` and now we can get root  