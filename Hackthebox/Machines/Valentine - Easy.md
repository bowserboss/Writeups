Started with a nmap scan
```
22 OpenSSH 5.9p1
80 Apache httpd 2.2.22
443 Apache httpd 2.2.22
```
Going to the webserver port `80` and `443` looks the same going to start a gobuster scan and see what else is one the host besides a picture
```
index.php
/dev
/dev/notes.txt
/dev/hype_key
/encode
/decode
/omg
```
Looking at the notes txt file it says they need to fix the `decoder/encoder` before going live and the `encoding/decoding` to be client side looking at the `hype_key` when we decode it from hexadecimal its a ssh private key we just need to add the headers running the `vuln` script with nmap says its vulnerable to `CVE-2014-0160` and that's probley what that key is for that we decoded this cve is for the heartbleed exploit using this poc https://github.com/sensepost/heartbleed-poc there is a python script we can use and its gonna give us the password for the `id_rsa` key we found `..................` to SSH into the box we have to do this command `ssh -o PubkeyAcceptedKeyTypes=ssh-rsa -i deocded-hex hype@10.10.10.79` and now we are the user hype if we run `ps aux` we can see root is running a tmux session and we can hijack it with this command `tmux -S /.devs/dev_sess` and now we are root 