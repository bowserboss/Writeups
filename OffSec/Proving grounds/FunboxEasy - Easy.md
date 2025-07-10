Started with a nmap scan
```
22 OpenSSH 8.2p1 
80 Apache 2.4.41
```
Since the only service we could exploit is a webservice lets start there going to the site we get a default apache page so going to start a gobuster scan
```
/store 
/admin ## admin login
/secret  ##this is just a quote 
/gym  ##fitness site
```
Looking at the store site and adding something to the cart and trying to buy it in the payment page there is a sqli injection we can exploit also for this site we can use the `admin:admin` and login into the admin account we cant seem to add a book it times out 

Going to the `Admin CRM` did a google search for some exploits and found a two for `small CRM` one is a auth bypass with SQL `' OR 'x'='x` and enter this for both username and password and now we are logged in cant do nothing with this tho
Gym site only has a login page everything thing else is a static site 

Going back to the store page searching for a exploit and found one and now we have a shell as `www-data` on the host looking into `tony` home folder and he has his passwords stored in there and we can read them `tony:yxcvbnmYYY` running `sudo -l` we have a lot of options I used the `pkexec` and got to root 