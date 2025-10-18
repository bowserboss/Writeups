Started with a nmap scan
```
22 OpenSSH 7.2p2
53 DNS
80 Apache 2.4.18
```
The DNS we cant do much with out a domain so going to the webhost its a default apache page so going to run a gobuster scan ran lots of scans could not find nothing so just  gonna guess there is a domain since there is DNS and we will try the name of the machine `cronos.htb` doing a DNS zone transfer we can see there is two subdomains `ns1,admin` `ns1` is nothing so going to the admin one we get a login page using sqlmap to test the login page using are cookie and it bypass the login page and went to `/welcome.php` which is a net tool that does traceroutes and pings and when we can get command injection after the ip we can add `;id` and we get `www-data` we can get a reverse shell with python3 and there is a `config.php` that has DB creds in it `admin:kEjdbRiggfBHUREiNSDs` we access the DB but the hash we find in there wont crack running linpeas we have a cron job running as root `php /var/www/laravel/artisan schedule:run` we can edit this file also so we can use this to get a reverse shell and we are root 