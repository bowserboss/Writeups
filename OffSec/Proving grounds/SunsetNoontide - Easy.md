Started with a nmap scan
```
6667 UnrealIRCD
6697 UnrealIRCD
8067 UnrealIRCD
```
With the nmap scan there is also a domain `irc.foonet.com` and using `xhcat` to connect to the IRC on port `6667` we see its running `UnrealIRCD 3.2.8.1` witch has a exploit for backdoor RCE metasploit has a module we can use and then get a better shell with a bash revshell and now we are the user `server` and this is the only user besides `root` on this machine I did alot of enumeration but found nothing we can exploit so just trying to `su` into the root account and tried a couple of passwords and `root:root` worked 