Started with a nmap scan
```
22 OpenSSH 7.9p1
80 Apache 2.4.38
```
When going to the site its just a pic of a anime person in a `jpg` file checked for steg stuff did not get anything so going to run a gobuster scan on the host 
```
/Cryoserver
/Temari
.Kazekage
/iamGaara
```
looking at the source code for this page and scroll down to the bottom we have three more pages we can look at but these other pages  just seem to be wiki about the person and we have a strong feeling that the username is `gaara` so we can try cracking the SSH password and it cracked `gaara:iloveyou2` doing some enumeration we have `SUID` bit set on `gdb` so we can go to GTFObins and use the suid exploit to get root and now we are root 