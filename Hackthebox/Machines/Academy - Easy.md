Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
33060 mysql
```
When we ran the nmap scan it shows the webserver redirects to a domain `academy.htb` did a subdomain scan did not find any going to do a gobuster scan now
```
/home.php
/login.php
/register.php
/index.php
/images
/admin.php
```
Going to the register page there is a POST parameter called `roleid` and we can change this to make us a admin account so we change the `0` to a `1` and now we can login into the admin page and there is a subdomain we get with a pending status `dev-staging-01.academy.htb` now when we go to the subdomain we can see its running `laravel` framework and there is a exploit for this `CVE-2018-15133` we can run the python3 script or there is a metasploit module we also need the api key using this we get to the user `www-data` if we go back a couple of folders to the `academy` and look at the `.env` file we get a password for the user `cry0l1t3:...........!!` and now we have SSH access running linpeas on the host we can use PwnKit to get root 