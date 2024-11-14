Started with a nmap scan
```
22 OpenSSH 8.3
80 nginx 1.18.0 PHP version 7.3.19
```
When going to the site there nothing but a 404 page so starting up a gobuster scan
```
/vendor
/phpinfo.php
```
When I sent the request to burp I found a domain and a subdomain `pwd.harder.local` when we use `admin:admin` we get a message saying `extra security in place code review will be done soon` 
Starting a subdomain scan only found two
```
pwd
shell
```
After running some scans a found on the `pwd` subdomain there is a `/.git/` folder using git dumper we can dump the `.git` folder 
`/.gitdumper.sh http://pwd.harder.local/.git/ /tmp/test` 
looking in the source code there is a `secret.php` and a `creditnal.php` files but we cant view them also there is a `hmac.php` file and there are some prams we need to set to get past this restriction we need to set `n` and the `host` pram we can set n to 1 and host to anything and we get a hash and if we set n pram we get a sha256 hash and now we can try and bypass the `$secret` if we use a `[]` so now if we use this url with the default creds we have we can get the login for the `shell.harder.local` 
`http://pwd.harder.local/index.php?n[]=1@host=test.com&h=(sha256 hash)` 
and we have creds now for the other site
`evs:9................................` now we can access the site but when we go to it says it can only be accessed by ip `10.10.10.x` so if we send the request to burp and add a `X-Forwarded-For:10.10.10.10` and we bypassed it if we look at the source code the pram is `cmd` and change the request to POST and now have code exaction doing some manul enumeration since I cant get a reverse shell if we look at the cron jobs we find a file called `evs-backup.sh` and it has the creds for the evs user with a password and now we can ssh into the host
`evs:.............................4` 
Now that we have a good shell running linpeas we found a root pub gpg key so we can import this key and then make a file to read the root flag and then sign the file and there is a binary called `exacute-crypted` and put are gpg file name and we have the root flag now 
