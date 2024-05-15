Answered some questions and then ran a nmap scan
```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.14.229
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
|_-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-favicon: Unknown favicon MD5: 1248E68909EAE600881B8DB1AD07F356
|_http-title: Smash - Bootstrap Business Template
| http-methods: 
|_  Supported Methods: HEAD GET POST OPTIONS
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Unix
```
The FTP has anon login looks like and has 2 interesting files 
Ok so now we have a web sever and it wants me to gobuster the website for a .php file
```
/.php                 (Status: 403) [Size: 278]
/login.php            (Status: 200) [Size: 1577]
/assets               (Status: 301) [Size: 315] [--> http://10.129.240.91/assets/]
/css                  (Status: 301) [Size: 312] [--> http://10.129.240.91/css/]
/js                   (Status: 301) [Size: 311] [--> http://10.129.240.91/js/]
/logout.php           (Status: 302) [Size: 0] [--> login.php]
/config.php           (Status: 200) [Size: 0]
/fonts                (Status: 301) [Size: 314] [--> http://10.129.240.91/fonts/]
/dashboard            (Status: 301) [Size: 318] [--> http://10.129.240.91/dashboard/]
```
So now we got a login page we going to brute the login and see what happends
since it was only 4 passwords I did it manually
```
admin:<Redacted>
```
Now we got the flag