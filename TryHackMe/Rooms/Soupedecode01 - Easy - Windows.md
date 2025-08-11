Started with a nmap scan
```
53 DNS
88 Kerberos
389 LDAP
3389 RDP
```
The nmap scan shows a host name for the PC  `DC01.SUPERDECODE.local` after adding this to the host file in all variations we go to testing the SMB and we find a guest account is in use and we can auth to the account with no password and we can READ the `$IPC` share so we can rid brute with this and get a list of usernames and we got a huge list of usernames now try to get a login as a user we can use nxc to test if there is any usernames and password as there username and see if we get any accounts and we got one `ybob317:ybob317` and with this account we can now access the Users share and we can check the desktop for the user flag and we go it now we can try and get user hashes with `impacket-getuserspns` and we can crack the hashes with hashcat and we got the login for `file_svc` `Password123 !!` and checking with nxc we got access now to the smb share of backup and there is a txt file with a bunch of hashes and users and we can test this with nxc smb and we got a hit on `fileserver$` and we can use `impacket-smbexec` to get on the server and we got the final hash 