Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
```
The nmap scan shows its running `backdroop cms 1` and looking at the about page there is a email with a `dog.htb`   running a gobuster scan to see if there is anything else on the site that's different from the github repo on this cms 
```
/.git/
```
and I found a `/.git/` folder and we can use git dumper to dump it looking in the source code now I found a email `triffany@dog.htb` with password `BackDropJ2024DS2024` and we get access to the site now time for reverse shell we can use this exploit `https://www.exploit-db.com/exploits/52021` to get a shell and install it as a module but we have to make it a `tar.gz` file and we can do so with this command `tar -czvf file.tar.gz shell/` and upload it manually to the site and now we can go to the link it gave us in the exploit and we can see are shell and we get a call back as `www-data` and looking at the web files we get a password in the `settings.php` file `BackDropJ2024DS2024` and its the same password and looking at the home folder we get two users 
```
jobert
johnusack
```
and we test the password and it worked for `johnusack` and now we can get the user flag when running `sudo -l` we have permissions to run `/usr/local/bin/bee` and we can use this to make backups and read the root flag 
`sudo /usr/local/bin/bee --root=/var/www/html eval "echo shell_exec('cat /root/root.txt');"` and now we have the root flag 