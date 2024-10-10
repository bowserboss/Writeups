Red Team 

Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
50000 web server 
```
started also a gobuster scan 
```
/index.php
```
Did not find anything on port `80` but on the port `50000` we get a team city login page and its running version `2023.11.3 build 147512` and there is a exploit for this on metasploit and we can get the flag for the user `ubuntu` 

 Blue team 
 Just have to look at the logs the package one cant be kinda hard to find just sort for Thursdays 