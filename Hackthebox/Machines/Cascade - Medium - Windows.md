Started with a nmap scan
```
53 DNS
88 Kerberos
135
139
389 LDAP
445
636
3268 LDAP
3269
5985
```
Domain `cascade.local` Hostname `CASC-DC1` 
We cant access SMB so moving to LDAP and we have anonymous bind so we can get a list of users doing a ldap search `ldapsearch -x -H ldap://10.10.10.182 -b "dc=cascade,dc=local"` and under `r.thompson` there is a attribute called `cascadeLegacyPwd` and this gives us a old password `rY4n5eva` looking in the SMB there is a share called `DATA` and inside in a couple of folders there is a `VNC Install.reg` that shows there is `TightVNC` and there is a hex password in this file for the user `s.smith:sT33ve2` and this user has winrm access going back to the SMB we try are new creds and find a new share we can access `Audit$` and there is a DB folder with `Audit.db` we can dump it there is also a exe file going back to this string we found in the db a google search decodes it for us to `arkSvc:w3lc0meFr3lng` and this user also has winrm access this user is also apart of the `AD Recyle Bin` so to enumerate this we can run this command `Get-AdObject -Filter 'isDeleted -eq $True -and name -ne "Deleted Objects"' -IncludeDeletedObjects` and we see one entry as `TempAdmin` we can look at the property's of this file `Get-ADObject -IncludeDeletedObjects -Filter {ObjectGUID -eq 'f0cc344d-31e0-4866-bceb-a842791ca059'} -Properties *` and we see the `cascadeLegacyPwd` and we can decrypt the password to `baCT3r1aN00dles` and this is the admin password we can now winrm into admin and get the root flag