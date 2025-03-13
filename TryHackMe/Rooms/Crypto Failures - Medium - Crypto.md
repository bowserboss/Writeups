Starting with a nmap scan
```
22 OpenSSH 8.9p1
80 Apache 2.4.59
```
Going to the site says we are logged in as guest and the cookie is protected with en"crypt" and when we view the source of the site says todo remove the `.bak` files so running a gobuster scan with with some `.bak` extensions and normal ones to 
```
/index.php
/index.php.bak
/config.php
```
looking at the `index.php.bak` its using the `crypt` algo that is unsecure and uses a 2 character salt we can bruteforce this since we know the cookie and we can see the site is looking for the admin user as well from the source code when looking at the cookie we can sperate it by the first to letters and then lets start a php shell and run this `echo > crypt("guest: Mo", "i9")` and its the same as are cookie so if we change the guest to admin and change the cookie and set the user as admin we get the first flag we can make a script to bruteforce the last part now since we know the secret key and generate a guest string with the user agent of `AAAAAAA` and we can get the last flag 