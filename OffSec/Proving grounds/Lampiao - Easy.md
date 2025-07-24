Started with a nmap scan
```
 22 OpenSSH 6.6.1p1
 80 http? 
 1898 Apache 2.4.7
```
Going to the apache server its running `durpal 7.54` so doing some google searching there is a exploit for this on metasploit `drupalgeddon2` and we can use this to get a shell as `www-data` going threw the webserver files and found a database login and the password for that works for the user on the machine also `tiago:Virgulino` running linpeas its vulnerable to the `dirtycow` exploit and we can use the `dirty.c` one and now we are root 



