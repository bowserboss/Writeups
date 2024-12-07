Started with a nmap scan
```
22/tcp    open  ssh     syn-ack ttl 63 OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)
80/tcp    open  http    syn-ack ttl 63 Apache httpd 2.4.10 ((Debian))
111/tcp   open  rpcbind syn-ack ttl 63 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100024  1          52971/udp   status
|   100024  1          55875/tcp6  status
|   100024  1          59140/tcp   status
|_  100024  1          59572/udp6  status
6697/tcp  open  irc     syn-ack ttl 63 UnrealIRCd
8067/tcp  open  irc     syn-ack ttl 63 UnrealIRCd
59140/tcp open  status  syn-ack ttl 63 1 (RPC #100024)
65534/tcp open  irc     syn-ack ttl 63 UnrealIRCd
```
Going to start with the webserver and run a gobuster scan on it 
```
/manuel
```
Did not find much on the webserver so I went to the irc services and connected to them its running `Unreal3.2.8.1` this version has a RCE vulnerability so we can use metasploit to get a reverse shell searching the host server for a little while I found that we can read this file `/home/djmardov/Documents/.backup` and it has a steg password in it `...............`  
the only image I could find that would make sense is the image on the webhost we run this command `steghide extract -sf irked.jpg` with the password we got and we got a file called `pass.txt` and we got the password for `djmardov` `...............` running linpeas there is a unknown suid binary called `viewuser` and it fails to load this file `/tmp/listusers` and we can make the file and then have it read the root flag