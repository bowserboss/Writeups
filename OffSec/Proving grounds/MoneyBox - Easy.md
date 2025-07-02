Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.9p1
80 Apache 2.4.38
```
The FTP does allow anonymous login there is a `jpg` file its just a cat pic so were going to move to the website now going to start a gobuster scan
```
/blogs
/S3cr3t-T3xt/
```
Going to the blogs folder says the site got hacked and in the source there is a secret folder when going to the secret folder and checking the source of the site agin that gives a secret key `3xtr4ctd4t4` could not find what to use this with tho so I tried to on the image we found we get a note saying `renu` has a weak password so we can try and use hydra to crack his ssh login and we cracked the login for ssh `renu:987654321` droping linpeas onto the host and lets see what we get we got nothing because it crashed but looking back at something I saw with the SSH keys it looks like `renu` `id_rsa` key can be used a `lily` and we can now SSH into that user and when we run `sudo -l` we can run `perl` as root with no password so going to GTFObins we can use the `sudo` exploit and now we are root 