Started with a nmap scan
```
22 OpenSSH 8.2p1
80 nginx 1.18.0
```
When going to the webserver its a `cacti` instance running version `1.2.22` there is a chained exploit for this to RCE and command injection there is also a metasploit module for this exploit `CVE-2022-46169` and now we are `www-data` running linpeas were in a docker container to get root we can run capsh with a suid exploit and now we are root looking at the `config.php` file we get the creds for the mysql database and we can run this command to show the user table `mysql -h db -u root -proot cacti -e 'select * from user_auth'` and we get a admin account and a account called Marcus and we cracked the password for `marcus:funkymonkey` and now we can SSH into the account if we look at the docker version and its vulnerable to `CVE-2021-41091` and now we can get root on the host machine 