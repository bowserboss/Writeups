Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
```
Looking at the website its a hacked site by `xh4h` and in the source code it mentions webshells so going to run a gobuster scan with the `-x php` option 
```
/smevk.php
```
If we take the comment on the source code of the page it will give us a github repo and if we take the names of the all the shells we will find one and then we can see the source code to get the login into the shell `amdin:admin` now we have access to the shell looking in the `webadmin` home folder we can add are ssh pub key into the authorized_keys file and now we have a real shell and looking at are `.bash_jistory` file we have some commands that were ran and there is also a `note.txt` that mentions that there is a tool to practice lua and in the history we have the commands we run `sudo -l` and sysadmin user is running a script that calls `privsec.lua` that does not exist we can make the file and script some lua code to read files as the sysadmin user and get the user flag running pspy we can see root is running a command on the banner of the ssh server every 30 seconds and we can edit this file as sysadmin so if we put some commands to cat the root flag to `/dev/shm` and then we can read the flag 