Started with a nmap scan
```
22 OpenSSH 7.6p1 (Ubuntu)
80 Aapache 
443 closed
```
The room also gives us a domain `http://enterprize.thm/` going to start with a subdomain scan 
```
maintest
```
When I went to the site it says `nohting to see here` go to mainetst tho its a `typo3 cms ` going to start a gobuster scan on the main site 
```
/public 403
/vendor 403
/var 403
/composer.json
```
running a gobuster scan on the subdomain 
```
/fileadmin
/typo3temp
/typo3 login
```
looking at the dates from a open directory it was installed in `1/03/2021` could be `version 11.0` or `10.0.9` going now to run the `typo3` scanner if we enter wrong creds into the panel it sets the site in a mode were it errors on everything and we found a file called `localconfiguration.json` and it has a hashing mode and secret key we can use php gadgets and using this link 
`http://maintest.enterpize.thm/index.php?id=38` is a form we can post to and then we can do these commands
`./phpggc -b --fast-destruct Guzzle/FW1 /var/www/html/public/fileadmin/_temp_/m3.php ../m3.php`
then we need to make a php script to make the hash we got into a gadget
`php hash.php` and then take the request to the forum and make sure to add the hash at the end of the first output and now we have code exaction now we can also we a reverse shell by downloading a sh file into the tmp folder now we got a call back as `www-data` user  in the `john` user home folder there is a file called `myapp` running a trace on this its calling a library called `libcustom.so`but we can make are own and compile it on are host and then putting it in the `/home/john/develop` and then make a `test.conf` file
`echo '/home/john/develop > /home/john/develop/test.conf'`
and now we can get a call back as john on are elf file or we can copy bash to his home folder and have them chmod it running linpeas agin all we found was the nfs we could not access as the `www-data` user so now we need to make the ssh file in are home folder and add a public key now we have a real ssh shell and we can portforward the nfs port and then we can make a suid binary and give it perms with sudo on are host then go to `/var/nfs` on there host and run `./shell -p` and now we are root