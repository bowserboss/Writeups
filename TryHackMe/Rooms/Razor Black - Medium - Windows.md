Started with a nmap scan
```
53 DNS
111 RPC
135 RPC
139 netbios-ssn
445 ds?
464 kpasswd5
3389 RDP
```
We got a domain name `raz0rblack.thm` looking for the nmap scan there is a nfs share running doing `showmount -e 10.10.210.31` and it list a `/users` folder so I mounted it and found 2 files called `employee_status.xlsx` and `sbradley.txt` the xlsx file is a list of names and there position in the company from looking at the txt file and the names gives us a possible hint to the username schema when running kerburte checking for valid usernames we got 3 valid 
```
lvetrova@raz0rblack.thm
twilliams@raz0rblack.thm
sbradley@raz0rblack.thm
```
and with this we can do some asrep roasting with the valid usernames 
`python3 GetNPUsers.py raz0rblack.thm/ -usersfile valid -format hashcat -outputfile hases -dc-ip 10.10.112.6` and we get one hash back now time to send that to hashcat and try and crack it and we have a valid login `twilliams@raz0rblack.thm:roast..............` with this we can auth to RDP, LDAP,SMB looking at the shares we can see now everything is no access but the IPC share so we can do a rid-brute and see if we get more usernames we got a new username `xyan1d3` re running the list with a known password we got from the other user there is one user that needs a password change its `sbradley` using `smbpasswd` we can change the password for the user 
`smbpasswd -r 10.10.32.224 - U sbradley` now we changed the password if this does not work Impacket also has a script we can use called `smbpassword.py` and now we can access the smb and there is a zip file in the trash share and a chat log saying its vulnerable to zero-logon so lets crack the zip file
`zip2john experiment_gone_wrong.zip > hash`
and we send it to john and got a password `electromag...........` and we have 3 files `system.hive` and `ntds.dit` we can use `secretsdump.py` to dump the hashes and we can also use `impacket-GetUsersSPNs` to get more hashes and we can crack this one as well and we got the password for the `xyan1s3` user `cyanide9.............` we also got the hash for the user `lvetrova:..........................` with the `lvetrova` user we can winrm into the host and get the flag we can run these commands to do so 
```
$Credential = Import-Clixml -Path "lvetrova.xml"
$Credential.GetNetwrorkCredential().password
```
and we got the flag now time to move to use `xyan1s3` which we all ready got the password for and we do the same thing to get the flag for the next user time to go for admin doing a `whoami /priv` we have the most enabled privs out of any other users we have access to and we have the `seBackup` privs so we need to get the dll files that go with it and upload them to the host threw the winrm next we need to import the files with `import-module .\SeBackuoPrivilegeUtils.dll` do this with both files now we need to download the `ndts.dit` file and get the SYSTEM file with these commands
`download C:\windows\ndts\ntds.dit` and then this command
`reg save HKLM\SYSTEM C:\Users\xyan1d3\SYSTEM` and download this file as well and then we can use a Impacket script called secretsdump and we got the hashes out of the files and can use the hash to auth as admin now we are admin the flag is in a `root.xml` file and we run the hash in cyberchef time for the final flag of Tyson going to his home folder there is a exe file with a super long name I downloaded the file and its just a txt file we can cat it out and the last flag 