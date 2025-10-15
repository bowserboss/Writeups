Started with a nmap scan
```
53 DNS
88 Kerberos
135
139
445 SMB
636 LDAP
3268
3269
5985 winrm
```
Domain `Megabank.local` Hostname `MONTEVERDE` 
On SMB null shares are disbaled and guest account but on ldap we can get null shares and get a list of users testing the usernames and passwords as the same thing gives us a account `SABatchJobs:SaBatchJobs` and looking at the SMB we have two shares that look intersting `azure_uploads` and `users$` using the `spider_plus` module `mhope` has a file in the `users$` called `azure.xml` with some creds in it `mhope:4notherD4y@n0th3r$` and these creds for ldap also SMB but nothing different and we have winrm access now and we got the user flag now now time to get bloodhound data we dont get much we see one interesting group `azure admins` group we can do this attack there is a tool online on github called `adconnectdump` we can use to dump the creds of the admin account and we got the creds `administrator:d0m@in4dminyeah!` 