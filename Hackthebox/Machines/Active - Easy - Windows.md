Started with a nmap scan
```
53 DNS
88 Kerberos
139 ssn
389 LDAP
445
464
593
3268 LDAP
```
Ldap does show a domain of `active.htb` looking at the smb we have null shares and we can READ one share called `Replication` to make this easy we can use the spider_plus module to quickly look at everything in the shares `nxc smb 10.10.10.100 -u '' -p '' -M spider_plus` and there is a XML file with creds in it hidden deep in the shares under Policies called `groups.xml` it has a username in it `SVC_TGS` and a `cpassword` which is Group Policy passowrd encryption in AES we can decrypt this with a tool called `gpp-decrypt` and then the hash and now we have the full login `SVC_TGS:GPPstillStandingStrong2k18` we can SMB as this user and get the user flag we can use netexec to test for kerberoastable accounts `nxc ldap 10.10.10.100 -U svc_tgs -P 'GPPstillStandingStrong2k18' --kerberoasting` and we get a kerb ticket hash for the admin user and we can use hashcat to crack this `hashcat kerb_user rockyou.txt -m 13100` and it cracked to `Ticketmaster1968` 