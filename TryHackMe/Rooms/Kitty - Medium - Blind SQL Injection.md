Started with a nmap scan
```
22 OpenSSH 8.2p1 Ubuntu
80 Apache 2.4.41
```
Started a gobuster scan
```
/index.php
/register.php
/welcome.php
/logout.php
/config.php
```
The login forum does not seem its vulnerable to Sql Injection but after some more testing it actually is this payload will bypass the login `' UNION SELECT 1,2,3,4 where database() like '%'; -- -` I made a script to brute force the database name the name of the database is `mywebsite` then we can use the script we made to brute the name of the tables as well the table name is `siteusers` now we can adjust are sql query agin and bruteforce the username section and we got a username `kitty` and now time same thing for the password `l0ng_li.......` the password works to login into the site took me a little bit to figure out what the next step is we needed to dump it as BINARY so we can match as byte to byte and now we have the password for ssh user `kitty:L0ng_Li.......` now we have access to the SSH server I found the Mysql creds `kitty:Sup3........` I was looking at the files for the website in the index.php file there is a X-FORWARD header that dumps the ip to a file called logged to access the site on port `8080` we can curl from the machine or forard the port via SSH and to see the apache settings we can run the `apache2ctl -s` command and we see a file called `dev_site.conf` so we can put a reverse shell in the header and get a call back as root