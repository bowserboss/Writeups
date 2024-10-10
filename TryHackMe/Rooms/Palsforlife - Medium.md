Started with a nmap scan
```
22 OpenSSH 7.6p1
6443 kerbnets 
30180 
31111 Git Tea
``` 
Going to the gittea instance we can make a account and login in looking at other users that maybe on this we find one `leeroy` with the email `leeroy@jenki.ns` 
Now to go to the webserver on port `30180` going to start  a gobuster scan since the webpage gives us nothing 
```
/team
```
The `/team/` page says there playing wow but looking at the source code of the page there is a pdf file with some encoded content after turning it from base64 to a pdf file its password protected so to turn this into a crack able hash we use `pdf2john` and to use hashcat to crack this takes the name of the pdf file out of the hash and then we need to use mode `10700` and cracked it to `............` decoding the pdf file looks like to be a password possible for the other user on the gittea lab `..........` and we can login to this account now time to get a shell access we can follow this exploit `CVE-2020-14144` and we can get a shell as `git` running linpeas I found some secret token and a `ca.key` file and we can use kebectrl and there is a 3rd flag in one of the pods and to get finale root we can make are own pod and exec are sell into a tty as `/bin/bash` and we are root 