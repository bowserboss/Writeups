Started with a nmap scan
```
22 OpenSSH 7.9p1
80 Apache 2.4.38
3306 MaraiDB 
```
The nmap scan shows that the webhost is redirecting to a site `http://sunset-midnight/` going to the site its a wordpress site so running a wpscan and we got the version `5.4.2` plugins `simply-poll-master` user name `admin` tryied cracking the account but  nothing and looking around the site was not finding anything with the plugins or core so going back to the mysql using nmap we can get a list of users `root,jose` and we can try and maybe crack these accounts and we cracked the root account `root:robert` we found a `admin` user with a has but we cant crack it but we can chnage the password with 
`Update wp_users SET user_pass = MD5('pass') WHERE user_login = 'admin';` 
and now we can login as admin time to get a reverse shell with the plugin exploit and now we are `www-data` checking out the `wp-config.php` file we can get the creds for `jose` running linpeas and we have this binary called `/usr/bin/status` and we can do PATH hijacking with this 
```
cd /tmp
echo "/bin/sh > service" 
chmod +x service
export PATH=/tmp:$PATH
/usr/bin/status
```
and now we are root 
