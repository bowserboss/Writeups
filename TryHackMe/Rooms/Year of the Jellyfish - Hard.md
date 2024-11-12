Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 5.9p1
80 Apache 2.4.29
443 Apache 2.4.29
8000 webserver
8096 webserver
22222 OpenSSH 7.6p1
```
The FTP does not have anon login enabled 
When going to the webserver port `80` redirects to `443` and does not have much going on on this webhost
Going to port `8000` its a devsite 
Looking back at the nmap scan there is some subdomains in the DNS part `monitorr` `dev` `beta` on the `monitorrr` one if we go to the settings page its shows us a database file in the source code got the database file and there is one login in it but we cant crack the password there is also a RCE un-auth exploit for what's running on this host `CVE-2020-28871` 
`beta` goes to the devsite and `dev` goes back to the main site so the only thing it looks like we can exploit is the monitor but both exploits wont work but we can modify the un-auth exploit we need to add the cookie to the script and to handle the self signed SSL cert when trying to get the reverse shell use port `443` and now we are `www-data` got the first flag in the `www` folder running linpeas it found we can use PwnKit or `dirty_sock` to get to root 