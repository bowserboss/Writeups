Started with a nmap scan
```
53 DNS
80 IIS 10.0
88 Kerberos
135 RPC
139
389  LDAP
445
3268 LDAP
3269
```
Domain `egotistical-bank.local` Computer name `SAUNA` 
Checking out the SMB we don't have null sessions or guest account's looking at the website there is a couple `html` pages the contact page sends a POST request but don't allow the method and looks pretty static ldap anonymous bind don't give us anything so we can try and find usernames with kerbrute `kerbrute userenum -d EGOTISTICAL-BANK.LOCAL --dc 10.10.10.175 jsmith.txt --downgrade` and we get a couple usernames `hsmith,fsmith` and for `fsmith` we got the kerb5 hash and we can try and crack it with hashcat and it cracked `fsmith:Thestrokes23` and now we have SMB access as this user we have access to one interesting share `ricoh aficio sp 8300dn pcl 6` but we only have write on this share testing some other stuff we got winrm access also and can get the user flag and we can run winpeas and we got the creds for `svc_loanmgr:Moneymkaestheworldgoround!` now we need to get bloodhound data and upload it looking at the data `svc_loanmgr` we have `GetchangesALL` so we can dcsync so we can use secretsdump to get the admin hash `impacket-secretsdump 'egotistical-bank.local/svc_loanmgr:Moneymakesthewordlgoround!'@10.10.10.175` and now we have the admin hash and can do pass the hash and winrm into the admin account 