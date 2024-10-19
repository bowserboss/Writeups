Started with a nmap scan
```
21 
22 OpenSSH 8.2p1
80 Apache 2.4.41 PHP 7.4.3
443 Apache 2.4.41 PHP 7.4.3
```
From the nmap scan we got some subdomains
```
blog ## did not respond
dev  ## this one only respondes on https site is PHP info
touch-me-not ## this one does not respond
netops-dev   ## this one is the firewall need to use https on this 
ball ## does not respond 
```
From going to the firewall and going to `firewall10110.php` we can see the UFW rules its allowing all the ports we found but the FTP going to check out the reset of the subdomains the only other one to respond at this time is `dev` and its a php info after playing around with the firewall I able to open some ports with `sudo ufw allow ftp` but did not open all ftp ports after trying a bunch of different ways I was just able to disable the firewall `sudo ufw disable` and now we can connect to the ftp with anonymous login and got `user.txt` there is a ssh key and we can ssh into the host as `challenger` I got the username from the `authorized_keys` file looking into the history file for the user we found 2 php files and in the `/var/www/notes/api/posts.php` and it has base64 encoded for the user cobra `mz4%o7BGum#TTu` with user can run `apt` as sudo with no password so we go to gtfoBins and use the 3rd option under sudo and now we got a shell   