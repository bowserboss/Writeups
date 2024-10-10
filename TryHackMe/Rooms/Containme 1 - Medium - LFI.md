Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
2222 EtherNetIP
8022 OpenSSH 7.7p1
```
Starting a gobuster scan on port `80` normal page is a default apache page  
```
/info.php ## phpinfo()
index.php ## direcotry listing on webserver files
/index.html
```
Going of a hint in the page source code in the `index.php` fuzzing for parameters and got one `path` and we do have LFI with this this payload we could see the passwd file `%0a/bin/cat%20/etc/passwd` one user `mike` now that we can start to see parts of the file system lets get the request into burp it will be easier to mess with in there use a php revshell I got a connection back now as `www-data` I had put linpeas on the host and found a interesting file in the man files called crypt `/usr/share/man/zh_TW` and when we do `./crypt mike` we get root access we did also have a `id_rsa` key for mike but we cant ssh into the server with it we found the host has 2 network cards and we can try to ssh into the other host but we need a password `172.16.20.2` mike did have a hash in the shadow file did not get anything so I start pinging the ip in the range and found another host `172.16.20.6` and we can ssh with mike into this host after doing some enumeration on the host there is mysql running and just giving it a guess we can access it with the username mike and password is `...............` and there is a database called `accouts` which has 2 users and 2 password in it one for root `....................` and another one for mike `WhatAr...........` and we use this passwords for root and in the root folder there is a `mike.zip` which takes the mike password and now we have a file called mike which is the final flag 