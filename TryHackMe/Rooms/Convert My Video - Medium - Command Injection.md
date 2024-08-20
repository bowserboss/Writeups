Started with a nmap scan
```
22
80
```
Started a gobuster scan
```
/imgaes
/admin
/js
/tmp
/tmp/downloads
```
The name of the secret folder is `admin` 
Messing around with the webpage and send the request to burp and if we take the data out and replace it just with `ID` we get a error that says run `youtube-dl` with `ytsearch:ID` looking it up on google its a python program also if we do `--help` it gives us the help output from the program maybe we can get command injection and after testing around for a bit got the command injection to work `|id;` to exacute commands with spaces we need to add `${IFS}` in the for the space after we got the flag we also seen there is a `.htpasswd` file in the `admin` folder 
`|cat${IFS}/var/www/html/admin.htpasswd` 
It has a username and password hash that we can try and crack make sure you get the right formats and whole hash I had forgot a `/` cracked it pretty quickly `itsme....in:j...e` we can use this to login into SSH but we can log into the admin section now and there is a button called clean Downloads and it send os commands from the site so we can just use the shell here its a lot better and possible get a reverse shell and lot easier I uploaded a php shell and stabilized my access you can get root access with PwnKit or just cat out the `root.txt` file with the `clean.sh` script that's in the tmp folder of the website and wait for the cronjob to exacute 