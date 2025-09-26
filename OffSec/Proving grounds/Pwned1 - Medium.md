Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.9p1
80 Apache 2.4.38
```
The FTP does not have anonymouse login so going to the site and we  get a hacked page there is a note in the source code that means nothing so running a gobuster scan and we find a `robots.txt` file with 2 folders 
```
/nothing
/hidden_text
```
`nothing` folder is nothing going to `hidden_text` we get a `secret.dic` file its a wordlist for gobuster and we get a hit on `pwned.vuln` and its a login page looking at the source on this page we get a login `ftpuser:B0ss_Pr!ncesS` and we cant login into the site but we can login into ftp and now we have a folder called `share` with two files in it `id_rsa` and `note.txt`  the note gives us a username of `ariana` now we can SSH into the machine and we got the first flag in the `/home` folder there is a script called `messenger.sh` and we can also run this as the user selena with `sudo -u selena /home/messenger.sh` and in the first question we can enter a `'` and we have command injection so we can do `/bin/bash` and now we have a shell as the user and from the linpeas script we can see selena is part of the docker group and if we go to gtfobins we can get a exploit for it and if we run  `docker run -v /:/mnt --rm -it apline chroot /mnt /sh` we are now root  
