Started with a nmap scan
```
21 vsftpd 2.3.4
22 OpenSSH 4.7p1
139 smb
445 smb
3632 distccd v1
```
Checking out the FTP service and there is no files on the FTP server now I'm going to run enum4lniux there is one share that says we can access and its `/tmp` on the tmp folder there is a `vgauthsvclog` googling the service version for the ftp shows there is a backdoor in this version and we can use metasploit to exploit it but the exploit does not work and googling the samba version and this exploit works and now we are the root user 