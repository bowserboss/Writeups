Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.9p1
80 Apache 2.4.38
```
Starting with the FTP since it allows anonymous login there is nothing on the FTP but a `pub` folder we can put files into going to the webserver now going to run a gobuster scan on it since its a default apache page 
```
/javascript
/manual
/robots-txt
```
Going to the `robots-txt` it says `you are not a search engine` so we google what Google's user-agent is when it crawls sites and its `Googlebot/2.1` for google Desktop and now we can read the robots file and got a hidden folder `/secret_information/` going to this it has a `index.php` page that uses a parameter so you can select the language you want `?lang=` and this is vulnerable to LFI and we can read the `/etc/passwd` file it has one user `tom` and a ftp user we can not log into trying to find the ftp path to the pub folder and found it `/var/ftp/pub/test1` and we can see the file I put on there so now we can get RCE with a PHP reverse shell had to use `base64` to get a reverse shell as `www-data` and in tom folder there is a script called `rootshell.c` and has a SUID bit set we can move this script to the `/tmp` folder make a file named `whoami` and have it echo the name `tom` `echo "tom"` and set the path to the tmp folder `export PATH=/tmp$PATH` and then go back to the `tom` folder and run the script and now we are root 