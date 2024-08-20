Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 8.2p1
80 gunicorn 
```
Starting with the website and clicking around we are auto signed into a account called `Nathan` and if we go to the security snapshot we load into scan 9 but if we change the 9 to a 1 we can load other scans so we need to scan this rage and see if there is any more we can access and pcap `0` has some info in it opening the pcap file it has a FTP stream in it and if we follow it there is a plain txt username and password and we can use this to log into the SSH service
`nathan:Buck3tH.....` 
Did a linpeas scan of the host and found a binary that we can exploit for root 
`/usr/bin/python3.8` 
and this binary we can exploit the `SETUID` cap to get root
`./python3.8 -c 'import os; os.setuid(0); os.system("/bin/sh")'`
and now we have root access 