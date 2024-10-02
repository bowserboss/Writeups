Started with a nmap scan
```
21 FTP vsftpd 3.0.3
22 OpenSSH 7.2p2
139 samba
445 smaba
1337 Apache 2.4.18
```
Staring with the FTP service it has anonymous login and has a pic in the pub folder going now to the smb with enum4linux it picked up on a user `chuck` and `ftpuser` after doing a nmap scan agin since I was not finding anything I found another port `1337` and its a web server when we go to it says were have been hacked then another prompt says they have left something here for us so I'm going to start a gobuster and see what we can find 
```
/index.html
/admin
```
There are some fake creds in the source code of this site so were still at dead ends going back to the picture when I ran `exiftool youfoundme.png` I seen the Owner's Name is encoded in something doing some google searching its `vigenere cypher` and the KEY is `birdistheword` from the youtube video we found and the pass decoded to `eas......` and this is for the chuck user for smb and now we can access the `nerdherd_classified` share and there is a txt file on the share called `secret.txt` and it has a folder path it gives us to the webhost `/this1.........................` and has a file in it called `creds.txt` and has SSH login for the user chuck `chuck:th1s41ntmypa5s` and now we are on the host I ran linpeas and its vulnerable to so much stuff so I just used PwnKit to get root and the flag is in the `/opt` folder the hidden flag is in the root `.bash_history` file 