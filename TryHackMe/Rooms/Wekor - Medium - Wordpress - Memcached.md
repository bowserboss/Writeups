Started with a nmap scan
```
22 OpenSSH 7.2p2
80 Apache 2.4.18
```
Since it have us a domain I'm also going to do a subdomain scan and found one `site`
Also while that's running starting a gobuster scan on the webhost since it has a black page
```
/index.html
/robots.txt
/comeingreallysoon
/it-next
/it-next/contact.php
/it-next/js
/it-next/config.php
```
Doing a gobuster on the `site.wekor.thm` domain since its also a blank page that says there will be a site soon
```
/index.html
/wordpress
```
And now we found the wordpress site and have something to look at going to do a wpscan on it the theme is `twentytwentyone` and is running version `1.0` and found one user `admin` going back to the other site were gonna go back and check the `robots.txt` file and there is some blank folders there was only one that worked `/comeingreallysoon` and this says go to `/it-next` and there is another site looking around it a bit I was testing every function with `'` and the coupon code section returned a error so I got the request in burp and sent it to sqlmap and and found a couple of databases the one we want is `wordpress` and we got some logins with hash's and we can crack 3 of 4 of them
```
wp_eagle:rockyou:eagle@wekor.thm
wp_jeffrey:soccer13:jeffrey@wekor.thm
wp_yura:xxxxxx:yura@wekor.thm
```
To login into the wordpress need to use the email not the username the `yura` account is a admin account and we can switch the theme to twenty twenty and change the `404.php` file to a reverse shell and we go to this link `http://site.wekor.thm/wordpress/wp-content/themes/twentytwenty/404.php` and now we have a reverse shell as `www-data` looking in the web folder in the `it-next` folder I found the creds for the mysql server `root:root123....` when we ran linpeas we see there is some open ports the one we want to look at is `11211` its memcached and we can enumerate this to get a password for the user `Orka` password is `OrkA.....` and now we are the user when we do `sudo -l` we can run a bitcoin program that's on are desktop and when you run strings on it we can get the password its `pass....` now if we do `echo $PATH ` we can see there is a `/usr/sbin` folder and there is not a file called python so lets make a file called python and put `/bin/bash` in it and save it then run the bitcoin program with root and now we are root user 