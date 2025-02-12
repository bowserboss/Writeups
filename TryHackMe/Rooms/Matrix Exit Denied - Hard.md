Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
3306 Mysql (MariaDB)
```
Now going to a gobuster scan
```
there is alot mainly all normal mybb files
/bugbountyHQ
/reportPanel.php
/devBuilds
```
The forum is running `MyBB` we can use `MyBBscan` to scan the site did not find much looking at the members on the site when I was looking around we seen one member has a pic of a white rabbit and the room told us to follow the white rabbit and then looking at there post its a bugbounty thread that goes to `/bugbountyHQ` and its a report forum that's been disabled and looking at the source code of the site found a `/reportPanel.php` and we can see all the reports and from the hint we know the year of the vulnerability was `x/x/21` and there is one that looks really good for password spraying attacks we can use passwords that are list on the description and we cracked two accounts 
```
ArnoldBagger:Luisfactor05
PalacerKing:qwerty
```
and there both mod accounts looking at the first account and reading all the messages I found another url path `/devbuilds` and it has 3 files called `.plugin` and a `p.txt.gpg ` and there running a plugin called `modManagerv2` and looking at the file there is a section were it links were it reads the sql password from `inc/tools/manage/SQL/p.txt` maybe this will have what we need to decrypt that txt file so we need to find a password that link did not work looking back at the report panel in the source code there is a key master message and we got 2 messages one is a folder and the other looks like password maybe but it was not so looking back at the new webpage we found looking at the source thee is a keymaker and Chinese letters translating comes back with nothing but looking at the code and hint we can make a python script to make permutation and then use `gpg2john`  and try to crack it and it cracked to `fvgoxq` and we decoded to a sql password `myS3CR3TPa55` from looking in all the files we had a username for the sql user `mod` so we can login now we have to disable SSL when trying to log in looking in the mod manager there is some `login_keys` and we find the one we need with all these login keys we can change are cookies and just put the user id in front `7_login_key` and we can auth now as the super admin `BlackCat` looking around this users profile we find a bunch of attachments looking at some of the documents we can see how SSH auth was going to work for the site and we can see a username `architect` and the login method is going to be SSH-totp we need to sync with the UTC time zone `timedatectl set-timezone UTC` and then we can unzip the `devtools.zip` and we can run the `ntp_syncer.py` and we also need to change the ip in the time sync file the other script we have needs to be fixed to include the proper country codes and now we can make a list of OTP codes need to use `mod_timesyn.py` script I made to get the ssh login and the secrets keys we need are in one of the pictures we can download now that were on the host gonna run linpeas se have permission over a binary called `pandoc` and we can use this to write files so we can copy the passwd file to are home folder and then modify it to add a password in the x section as root and then run this command 
`/usr/bin/pandoc passwd -t plain -o /etc/passwd` 
and then su into root for the pin we can read the `/etc/bigpual.txt` and we get a password to find the root flag there is a root python file in the `/etc/` folder so if we run this command we can run the file `find . -name '--*' -exec python3 {} +` and now we got the root flag 