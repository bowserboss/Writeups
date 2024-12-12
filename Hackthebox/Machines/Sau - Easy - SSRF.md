Started with a nmap scan
```
22 OpenSSH 8.2p1
80 
8338
55555 unknown http server
```
Going to the webserver its a `request buckets` instance when googling for exploit found a SSRF exploit `CVE-2023-27163`  we can use this exploit to hit the internal ports of `80` to expose another web instance running `Mailtrail 0.53` there is a metasploit module to help with this  and we get a call back as the user `puma` to get a stable shell we can make a `.ssh` folder and make a authorized keys file and add are pub key to it and ssh into the host as `puma` if we run `sudo -l` we can run `systemctl` as root if we go to gtfo bins and run the command and then do `!sh` and now we are root 