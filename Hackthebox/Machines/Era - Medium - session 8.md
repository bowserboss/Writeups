Started with nmap scan
```
21 vsftpd 3.0.5
80 nginx 1.18.0
```
The nmap scan shows a redirect to a domain `http://era.htb` but checking out the FTP first and it does not have anonymous login so going to the website now its a static website and the contact does not load anything also so going to do a subdomain scan now and found one subdomain `file` there is a login page and a page to reset your password we can use the password reset to enumerate usernames that are registered 
```
eric
john
ethan
veronica
yuri
```
now we have a list of the users on the webapp going to also run a gobuster scan and see what's on the webhost 
```
/images
/download.php
/login.php
/register.php
/files
/assets
/upload.php
/LICENSE
```
Looking at the login page there is only a login and from the main page there is also a login with security questions but from the gobuster scan we see there is a `register.php` and we can go there and make a account and now we can login into the webapp and looking around we can find the upload page that we also found with the gobuster scan and we can upload anything to this so I opened up caido and uploaded a file and it gives us a download link and looking at this link it has a `id=9252` it gives it a number we can test this for IDOR and see if we can find any other files we can test this I started at the first 1000 numbers and found 2 files we could download a `signing.zip` and a `site-backup.zip` site backup looks more interesting so lets download it and unzipping this file we see there is a database in here also called `filedb.sqlite`  we can dump this database with `sqlite3 filedb.sqlite .dump` and we can see the users and there hash and the users in here are the same ones we enumerated and a admin user with random letters and number after its name so lets see if we can crack any of these hashes and we cracked two users `yuri:mustang` and `eric:america` we can use these logins and we have a option to reset security questions for any user and we can reset the admin user and go back to the main page and use the security question login and now we are `admin_ef01cab31aa` user and we can see the two files we found from the IDOR in the `download.php` there is a another parameter `format=` in the download url and this is vulnerable to SSRF and command injection `format=ssh2.exec://eric:america@127.0.0.1:22/(command)` 
and we can use this to get a revshell if we host a file and then have it exacute we can find this out by FTP into the host as `yuri` and we have the apache config and php config but going back to the url we can get a reverse shell with this using bash and base64 url encoded and now we are `eric` on the host running linpeas we can see is running some AV binary called monitor and when we run pspy we can see the process and the command its exacting and we can use these same commands and make a backdoor to get a reverse shell 
```
gcc b.c -o backddor
objcopy --dump-section .text_sig=/opt/AV/periodic-checks/monitor
objcopy --admin-section .txt_sig=txt_sig backdoor
cp backddor monitor
chmod +x monitor
```
sometimes we will get that the file is busy and have to wait a little bit to run the commands but we will get a callback as root 