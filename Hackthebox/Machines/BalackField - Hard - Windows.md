Started with a nmap scan
```
53 DNS
88 Kerberos
135
389 LDAP
445
593
3268 LDAP
5985 winrm
```
Domain `blackfield.local` Hostname `DC01` 
On SMB null sessions do not work but guest account does and we can rid brute to get a list of users since `--users` wont work and the smb does have a interesting share `profiles$` has lots of folders but no files in them there is also a `forensic` share we dont have access to yet did a couple bruteforce attacks with the usernames as passwords did not get anything then I ran kerbrute and got a hash for the `support` account now we need to crack the hash with hashcat and we cracked the account `support :#00^BlackKnight` and checking the password policy now there is account lockout of `30 minutes` we don't have access to any new shares so now time to get bloodhound data looking at it `support` has `forcechangepassword` over the user `auit2020` `net rpc password audit2020 "newP@ssword2025" -U blackfield.local/support%'#00^BlackKnight' -S 10.10.10.192` and now we have access to this account and it has access to the `forensic` shares this account also has no outbound connections looking in the shaes we found a `memory_analysis` and a `lsass.zip` which has a dump file we can use `pypykatz` to extract the info and we get a bunch of accounts but the `svc_backup` works with the NT hash and we have winrm access now and we have user flag now doing a `whoami /all` shows we are apart of the `SeBackupPrivilege` group we can make a show copy of the PC and get cred from the ntds of the machine 