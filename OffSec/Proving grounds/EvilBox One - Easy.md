Started with a nmap scan
```
22 OpenSSH 7.9p1
80 Apache 2.4.38
```
Going to now start a gobuster scan
```
/robots.txt  says Hello H4x0r
/secret
/secret/evil.php  has nothing on it
```
The only thing we can find that might be something is the `/secret/evil.php` we can test this for pram fuzzing `ffuf -w /opt/Seclists/discovoery/direcoty-list-lowercase -t 60 -u http://$IP/secret/evil.php?FUZZ=/etc/passwd` and we get one hit `command` and we got LFI and there is one none root user `mowree` and we can check if there is a `id_rsa` key in there SSH folder and there is we can crack it using `ssh2john` and it cracked to `unicorn` now we can SSH into the host and get  the first flag running linpeas we find we can edit the `/etc/passwd` file and use this to get root first we run `openssl passwd 124` and then edit the passwd file take the x out and add the hashed password and now we can run `su` and we are root 