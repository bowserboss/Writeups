Started with a nmap scan
```
80 Apache 2.4.51
135
445
10000 MiniServ 1.981 webmin
20000 MiniServ 1.830 webmin
```
Looking at the webmin first and did not find anything interesting so going to port `80` and its a default apache page looking at the source of the page if you go to the very bottom there is a message that says its encrypted and its there access its encoded with something called brainfuck and we can decode it to this `.2uqPEfj3D<P'a-3` now checking out the smb if get a username of `cyber` and now we can log into the `usermin` panel and at the bottom of the page there is a option for us to run system commands so now we have access to the machine trying to run linpeas on the host and we cant so we have to Manuel do enumeration and there is also a file in are home folder called `tar` and looking around I ran the get caps to find what programs have caps and the tar program does  so we can do some googling and there is a guide there using the tar binary and we got the root flag 