Started with a nmap scan
```
21 vsftpd 3.0.3
61000 OpenSSH 7.9p1
```
Starting with the FTP since thats all we have it does allow anonymous login there is a folder called `.hannah` and it has a `id_rsa` key in it and it has no password so we can just SSH into the host on port `61000` and got the first flag and looking at binary permissions we have one called `mawk` and we can use the `suid` exploit on GTFObins to read the root flag  