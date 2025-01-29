Started with a nmap scan
```
22 closed SSH
80 Apache 2.4.38 (Debian) 
```
Looking at the name scan and the HTTP title it has a domain `Hackfail.htb` so we add it to are host file and now we can see the website going to start with a subdomain scan found one `dev.hackfail.htb` we can use a tool called `eos` and help us the find the source code files and the register is checking to see if the username does not match `elonmusk` and we can register this username if we just change the capitol letters `ElonMUsk` and when we log in we are now admin user and can read the tickets and looking at the `/download` and doing some testing on the parameter  and we got LFI and we can write a script to automate this and now we can read `/etc/passwd` when looking at the `.env` file we can see its running `symfony` and there is a script on github that will help us exploit this to get a reverse shell and we get a call back as `www-data` 
running pspy we can see there is a process thats running a java file and we can download it and run it in `jd-gui` and see its useing the same ip we saw on pspy `172.22.1.250` on port `1099` so we know there are other host we need to get to so we need a static binary of nmap to put onto the host running this command `nmap -sn 172.22.1.1-255` we find 3 hosts on the network
```
172.22.1.53 
21 ftp

172.22.1.97

172.22.1.250
1099 rmi
1337 unknown
```
doing nmap scan on the 53 host shows we have ftp open and there are 2 ports open on the `250` host we can use chisel to port forward we can use a program called `rmg` its a vuln scanner for the `rmi` service we also need to set up are proxychains so we can scan the host it did not find anything so we can try and use the guess now and there is a login and a `senddata` the login has no function but with the `senddata` we can make a yso payload and now we are the monitoring user on the `250` host and the flag is in the home folder from this host we can connect to the `53` host and access the ftp we saw with a `anonymous` login and there is a file called `backup.tar` that has a `logs.txt` file and it looks like a process list log and it shows the user `elonmusk` connecting to a mysql service with a password `28fL+PvkSl0P5+zhkvPLCw` ands database `appli` but we gonna try the ftp first with this login and it works we have 2 files `hackfail.ovpn` and ` hackfail-auth.apk`
getting the apk file back to are host and decompile it we in the `MainActivity` there is a list of decimals and its using `chacha20` for encryption doing some googling I found a article on how to decrypt it and had chatGPT help me make a python script and when we decode it we got the next flag now back to the apk file we need to run it in a emulator because when we try to connect to the openvpn file we wants a password and when we load the apk its also asking for a password and its the flag from the last answer and now we get a opt code and that's the passowrd for the vpn and now we are auth to the vpn and now we are on another part of the network and lets use fping to see what host are on here 
```
172.22.0.6
172.22.0.1
172.22.43.1
172.22.43.142
```
nmaping the `43.1` host its running a squid proxy on port `3128` when going to the web proxy at the bottom of the page and says `core01` so we can take a guess that the domain is `core01.local` and we can use a tool called `spose` to scan threw the squid proxy 
`python3 spose.py --proxy http://172.22.43.1:3128 --target core01.local --allports`
and we found one port `22` running SSH and we need to configure a tool called `corkscrew` so we can SSH into the host but we need creds still so going back to the ftp there was a folder called bob and in there is a readme file that says bob was supposed to change his creds for the network admin so trying those creds `network_admin:network_admin` we can ssh into what looks like a router and run the flag command and we got the next flag
`ssh -o KexAlgorithms=diffie-hellman-group14-sha1 -o Ciphers=aes256-ctr -o MACs=shmac-sha1 network_admin@core01.local`
and if we read the changelog and says stuff about shell escapes so trying a couple different ones I found `|$id` works and then we can use netcat to get a reverse shell as `network_admin` and we got the next flag running `sudo -l` we can run `admin_backup` as root with no password looking at the binary and the code we can run `sudo admin_backup backup --conf-file=/etc/passwd` but we can only read the first like and doing some more looking around running netstat we can see `erland` owns a cookie and there is a exploit we can do with this cookie to get root user 
`sudo admin_backup backup --conf-file=/root/.erland.cookie`
and run the python script and now we are root and got the next flag and if we go to the `/` folder there is another flag but we cant read it so if we check are perms there is `pkexec` so lets try the `2021-4034` exploit and now we can read the flag

Flags:
1. after reverse shell can find the flag in the blog folder on web root 
2. flag in home folder as monitor on `250` host
3. decode the hex decimal blob in the apk file 
4. ssh into core01.local with creds and run flag
5. shell escape with `|$` to get reverse shell 
6. get root 
7. exploit pkexec