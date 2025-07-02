Started with a nmap scan
```
80 Apache 2.4.18
4512 OpenSSH 7.2p2
```
Going to the site its running `Wordpress` so we going to use `wpscan` to see what we can find its running version `4.1.31` and its using theme `twentyfifteen` and we got three users
```
hugo
philip
c0ldd
```
going to also run a gobuster scan on the site
```
/wp-content
/wp-admin
/wp-includes
/hidden
```
When going to `/hidden` it says `c0ldd has changed hugos password` so going back to the login page we can try a bruteforce the password we can use `wpscan` for this after a little bit we cracked the password `password123456` but we cant do much with this user lets see if we can get access to the `c0ldd` user and we got there password now `9876543210` and this is the admin account we can edit a `404` page with a simple shell that we can exacute commands and now we can exacute a reverse shell and we get the user `www-data` and there is a `c0ldd` user we can get the first flag going back to the website files looking at the `wp-config.php` file we see the password for the database and its also a user `c0ldd` trying this password for the user account and we can login `c0ldd:cybersecurity` running `sudo -l` we have three options to get to root I chose the FTP and going to GTFObins and use the sudo exploit to get to root and now we are root 