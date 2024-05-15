Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29 
```
Starting a gobuster scan with the wordlist `2.3-medmium` 
```
/images
/themes/
/uploads
/css
/cs
/fonts
```
While this was scanning I was looking at the source code and found a `js` file and found the login path for the web site
	`cdn-cgi/login`
So we can login in as a guest and I found I `IDOR` in the account section we can change the url to `id=1` and get the admin username and access id then we go to the `uploads` section and change are cookie id to the admin id `34322` and we can access the uploads section now and we can upload a file and it goes to the `uploads` section and we can download it from there so maybe a file upload to reverse shell can be done but first we going to try and get a list of all the users on the site using burp and the `idor` from eailer 
```
1 - admin id of admin 34322
4 - john
13 - Peter
23 - Rafol
30 - super admin id numer 86575
```
After uploading a php reverse shell got a call back to my machine we go to the `/var/www/html/cdn-cgi/login` with this command `cat * | grep -i passw*` and in the code of one of the files we found a username and password `admin:MEGACORP_4dm1n!!` and if we cat the `db.php` file we get the login for `robert:M3g4C0rpUs3r!` using the find command we can find the files that are group can read 
	`find / -group bugtracker 2>/dev/null`
and we find the `/usr/bin/bugtracker` are group can exacute looks like this file is using the cat command to read a file so we go to the `/tmp` and make a file called `cat` and then set the permission `chmod +x cat` and then we have to set the path`export PATH=/tmp:$PATH` exacute the bugtracker again with id of `2` and now we have root