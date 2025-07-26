Started with a nmap scan
```
80 Apache 2.2.22
```
The nmap scan shows there is a `robots.txt` and it says `make sure to add .zip to the dir brute` and it has a folder which has a login page so were giong to run a gobuster scan on the main web root 
```
/index
/db
/robots.txt
/smapper.zip
/textpattern/textpattern
```
The `zip` folder is password protected and we can use `zip2john` to get the has and then use hashcat to crack it and it cracked to `myspace4` and we get a file called `creds.txt` `mayer:lionheart` and we can use this to login into the website its running CMS `textpattern CMS` version `4.8.3` there is a exploit for this but I could not get it to work so I did it manually and found the upload file section under files and found the link to find them `/textpattern/files/t.php?efcd=id` and we got code exaction `www-data` and we can get a reverse shell using that and its vulnerable to the `dirtycow` exploit and we can use it to get root   