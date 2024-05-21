Did a full nmap with out port scan
```
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
8080/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
|_http-title: Simple Image Gallery System
```
possiable sql inject in the id on the cms

on the login page on `8080` use sqli to login as admin input this as username and password ' or 1=1 -- -

found on the id pram on the albums was vulnerable to sqli injection and got the database

cant crack the password for admin

so we upload a php file as a jpg file and get a rev shell upload php file that jpg in albums 

stablize rev shell
	`python3 -c 'import pty;pty.spawn("/bin/bash")'`

something entersting maybe
	`sudo -l<Redacted>`

	`mike:<Redacted` su login

mike can run a sh script as root stablize rev shell to use nano and use the gtfobins to exploit and get root
