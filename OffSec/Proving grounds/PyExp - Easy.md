Started with a nmap scan
```
1337 OpenSSH 7.9p1
3306 MariaDB 5.5.5-10.3.23
```
So all we have to enumerate is mysql there no exploits we can use they all super old or need creds so we can use nmap to try and enumerate some users and we got back 10 users
```
root
netadmin
guest
user
web
webadmin
sysadmin
administrator
admin
test
```
now we can use hydra and try and crack them and we cracked one user `root:prettywoman` and going into the databases there is one interesting one called `data` with a table called `fernet` and inside that is a key and cred and doing some googling fernet is a python encryption and we can make a script to decrypt this and we get a login to SSH since thats the only other service 
```
lucy:wJ9`"Lemdv9[FEw-
```
and we can SSH with this login and get the user flag when running `sudo -l` we can run a python script we cant edit it but its using a dangerous function exec() and if we run the script and then enter `__import__('os').system('/bin/bash')` and now we are root 