Started  a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.6p1 
80 Apache 2.4.29
```
Going to start with the webserver when doing in nmap scan there is a listing in `robots.txt` called `page0....456.php` when leads us to a LFI 

Also going to check the ftp since it has anonymous login and 2 files on it called `note` and `remenber_this` from the LFI we saw and for the notes on the ftp we can find which user has a weak password
So we got to hydra and try to crack ssh with both of the users we can see `sophia` and `alex` we cracked `sophia` login `ba....rl` so now we can login into ssh with this and then we can go to alex home folder and find he has the flag but we cant read it so we need to become alex or root
Looking around a little bit I find a folder called in alex that has his password `alexis......23`
so now we are the user alex and we run `sudo -l` and we can run curl as sudo so using this exploit we can get root
```
echo "alex ALL=(ALL) NOPASSWD: ALL" > /tmp/sudoers
echo "Defaults!ALL !requiretty" >> /tmp/sudoers
sudo /usr/bin/curl -T /tmp/sudoers file:///etc/sudoers.d/malicious
sudo su
```
and we got root

