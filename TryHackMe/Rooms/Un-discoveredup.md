Started with a nmap scan
```
22 OpenSSH 7.2p2
80 Apache 2.4.18
111 rpcbind
2049 NFS
38905 nlockmgr
```

Doing a gobuster scan
```
/imgaes
```
Starting a nikto scan
```
/images
```
Started a fuzz for vhost found a bunch
```
dashboard
deliver
newsite
manager
develop
network
forms
maintenance
view
mailgate
play
start
booking
terminal
gold
internet
resources
```
Out of all these there is only one that's different its `deliver` so going to start a gobuster on it and see if there is any files on this one there is not on the others
```
/media
/templates
/files
/data
/cms
/js
/LICENSE
```

They all seems to go to the same site which is running `RiteCMS` version `2.2.1` which does have a authenticated RCE  

Going to the `/data` there is a file called `content` and it says there is a todo list `crate nfs share for william on /home/william` but since we don't know his `uid` we cant mount the share so I tried using hydra on the login page for the cms and got the admin account to crack
	`hyrda -l admin -P rockyou.txt deliver.undiscovered.thm http-post-form "/cms/index.php:username=admin&userpw=^PASS^:F=user unknown or password wrong"` 
	Login
	`admin:liv.....l`
After we login in there is a file manager and we can upload a php reverse shell and go to the `/files` and we get a call back to are host as `www-data` there is 2 users `william` and `leonard` 
just went right for root with PwnKit 