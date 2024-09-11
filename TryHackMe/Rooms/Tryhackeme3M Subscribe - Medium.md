Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
40009 Apache 2.4.41
```
Started a gobuster scan on the webhost on port `80` 
```
/img
/login.php
/index.php
sebscribe.php
/css
/js
/logout.php
/config.php
/dashboard.php
/sign_up.php
/phpmyadmin
/connection.php
```
So when I was looking at the sign up form there is a js file that gives a response when sending a request and gives us another domain `capture3millionsubscribers.thm` that will give us the code if we go to the sight up page and go to console and exacute the `e()` function and it gives us a code of `VkXgo:Invit........` after we give the token to the site it sends us creds for the guest account `wedidit.....` and when looking around the site I change my vip status in the cookies and then accessed the vip room and it sends a request to a strange url that turns out it takes command input onto a host as `www-data` when looking at the code of the file we can only run 4 commands run, whoami, ls, cat and we can access a file called `config.php` and get the access token for the admin panel and the url token is `ACC#SS........` and url `admin1337special.hackme.thm:40009` when going to the website it gives us a 403 forbbiden but if we add `/login.php` we get a page that ask for the access token and gives us a login page and looking at the source code of the page did not see anything that really stound out so I took the request and started sqlmap while I was still looking around and sqlmap found a injection in the username field and now we can dump the hackme database and now login into as admin and we can manage sign up and invite code but it looks like it does nothing but when we go back to the main site it gives us the last flag 