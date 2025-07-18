This room gives us creds to something `samuraiseppukutanto/fakepaswordsitstolong` 
Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.9p1
80 nginx 1.14.2
139 smb
445 smb
7080 Lightspeed
7601 Apache 2.4.38
8088 Lightspeed
```
Started with the FTP but we cant login 
Went to the SMB and the the guest account is enabled and the login works but treats it as a guest account and we cant READ any shares 
When going to port `80` we get a login page that we cant login into 
Going to webhost on port `7601` and it looks the same as what's on port `8088` and running a gobuster on this we get a lot of empty folders 
```
/secret
/keys
/production 
/production/assets
/production/forms
/production/changelog.txt
```
There is a website at the `production` the `secret` folder has a password list and a `passwd/shadow` files and there is one user `rabbit-hole:a1b2c3` and we was able to crack the password but this login also does not work for anything we have so far 
Going to the `/keys` is a `id_rsa` key it says it has no passowrd and we cant SSH into the host with the 2 users we have
Going to the next webhost on port `8088` it looks the same as the other site but its not going  to run a gobuster on this site as well 
```
/docs
/cgi-bin
/blocked
```
So after lots of enumeration I could not find something we could use so trying to crack the SSH account with a couple of different usernames and we got one to crack `seppuku:eeyoree` and now we have access to the server and there is two other users `samurai,tanto` we can use the ssh key to get access as the `tanto` user and we can get access to the other user when we `sudo -l` as `samurai` there is sudo perms for a file that does not exist on the tanto user so we can make the file and make a bash script that has `/bin/bash` in it but we need to ssh with `bash --noprofile` so we can get out of the `rbash` and now we go back to the samurai user and run the sudo command and we are root now 