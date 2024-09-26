Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
139 Samaba 
445 Samba Ubuntu
873 rsync
```
Gonna start with the smb and see what we can get there first did not find any shares with the guest account
Starting a gobuster scan 
```
/index.php
/admin
inde.html
```
Going to the `/admin/` section and reading the note left on the webpage it says `make sure admin functionlituy can only be used in dev enviroment` this seems like a dead end for right now so lets try and enumerate the rsync  we can do some enumeration with nmap and found one module `Conf` the rsync version is `31.0` we can list all the modules with this command `rsync -av --list-only rsync://10.10.219.96:873/Conf` and it will list all the stuff in the config and to download all the files so we can read them `rsync -av rsync://10.10.219.96:873/Conf ./rsync/` and looking at the files the `webapp.ini` file is set prod and we need to set it to dev so we can edit the file and then upload it back to the server `rsync -av (path to file) rsync://10.10.219.96:873/Conf` and now the `webapp.ini` is on the host machine now we can view the `/admin/` section of the site it ask for a username and we can submit a query and its vulnerable to SQL injection looking in the db there is one user `tom:the...t` but that's the same login as the webapp config but we can try to get a shell with sqlmap with `--os-shell` and we got one and then we can download a rev shell script and then get a better reverse shell there is something on port `1027` but we can also just use `PwnKit` to get root 