Started with a nmap scan
```
22 OpenSSH 7.9p1 Debian
80 Apache 2.4.38
```
Going to the site its just a blank page that says the name of the machine so going to start a gobuster scan
```
/wordpress
```
Now we found the main site going to run wpscan on the wordpress installation wordpress version `5.5` theme `twenty twenty version 1.5` users `admin` plugin `social warfare version 3.5.2` and this has a RCE exploit `CVE-2019-9978` and if we use the system function from the PHP we can run system commands `system('ls');` and we can also use this to get a reverse shell and now we have access to the `www-data` account looking in the `wp-config.php` file there is a mysql login but it does not work but the password does work for the user account on the system `takis:R3&]vzhHmMn9,:-5` now if we run `sudo -l` we can just su into the root user `sudo su` and now we are root 