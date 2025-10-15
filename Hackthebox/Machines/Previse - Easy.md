Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
```
Looking at the website all we have is a login page running a gobuster scan found a `nav.php` file when intercepting the request with burp we can see the `accounts.php` page we can see there is a section to make a account now by default we need to edit some settings in burp we need to make it not skip redirects match the `content type header` and then in the match the replace to changed `302 Found` to `200 Found` and now we can view the page and make a account now we can login and there is a backup of the site looking at the backup we have the code for the site and on `logs.php` file we can get blind command injection we can control the `delim` parameter `;ping 10.10.16.4 -c 1`  and start tcpdump and we get a ping back so we can get a reverse shell now and we get a call back as `www-data` when getting a shell use python3 shell everything else seems to break the site and since we all ready had the `config.php` file we have the mysql creds `root:mySQL_p@ssw0rd!:)` and after logging into the mysql there is a table called `accounts` and a user called `m4where` and its a wired hash has a salt shaker in the hash but we can still crack it turn out its `md5crypt` and it cracked `ilovecody112235!` and now we can SSH into the host and get a real shell running `sudo -l` we can run a script as root `/opt/scripts/access_backup.sh` we can control the `gzip` binary like this
```
cd /tmp
export PATH=/tmp:$PATH
echo -ne '#!/bin/bash\ncp /bin/bash /tmp/bash\nchmod 4755 /tmp/bash' > gzip
chmod +x gzip
```
and then `./bash -p` now we are root 