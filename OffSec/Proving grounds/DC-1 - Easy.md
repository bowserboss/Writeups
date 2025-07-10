Started with a nmap scan
```
22 OpenSSH 6.0p1
80 Apache 2.2.22
111 RPC
52657 RPC
```
Going to the website its running `Durpal 7 cms` we can use the `drupalgeddon2` exploit with metasploit  could not get it to work so I used the PoC from `exploit-db` and we can use this to get a reverse shell and we get a call back as user `www-data` looking in the `sites/defualt` folder there is a `settings.php` file with the creds for the database `dbuser:R0ck3t` and we get two users and there hash lets try cracking them but there are only two users `flag4,root` and `flag4` has no password set so we cant get to this user doing some enumeration we find we have `SUID` prives on the `find` command and we can go to GTFObins and use the shell exploit and we have root 