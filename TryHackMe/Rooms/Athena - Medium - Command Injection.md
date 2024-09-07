Started a nmap scan
```
22 OpenSSH 8.2p1
80 Apache http 2.4.41
139 Samba 4.6.2
445 Samba 4.6.2
```
Looking at the smb first we found 2 shares
```
public
IPC$
```
In the public share there is a msg saying
```
I would like to inform you that a new ping systme is being developed and I left the corresponding application in a specific path, which can be accessed from /myrouterpannel
```
If we go to the webhost its apart of the site says we can ping other devices if we put in a ip address and gives us the out put of the ping command and it is vulnerable to command injection with this payload we can read `/etc/passwd` `%0Acat%2fetc%2fpasswd` I finally got a reverse shell as `www-data` I ran linpeas and there is a `backup.sh` file in the `/usr/share/backup` and we can edit this file and put a reverse shell in it and its running on a cron job and now we are `athena` and they have a `.ssh` folder with a `id_rsa` in it if we run sudo -l we can run insmod and its calling as file called `venom.ko` doing a google search turns out its a root kit and run it and tried to kill the process its supposed to spawn but nothing happened so I got it to my machine and opened it up in ghidra and they changed the process number so if we kill the right one we become root user 

Users on the server 
```
athena
ubuntu
```
