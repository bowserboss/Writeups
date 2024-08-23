Started a nmap scan
```
22 OpenSSH 8.9p1
80 nginx 1.18.0
```
From the namp scan we can see we have a domain called `usage.htb` so need to add that to the host file and the only thing open besides SSH in the webserver so lets check that out 
Going to start with a subdomain scan
```
admin
```
Running a gobuster scan on the website did not find anything
Looking at the admin section of the site I see that's its in Laravel php framework that's vulnerable to SQL injection in the `/forgot-password` in the email function of the POST request its a boolen-based blind injection I used  `sqlmap` with risk and level set to 5 and we got the database name now `usage_blog` and we want to go after the `admin_users` table and we got the admin user hash time to decrypt it `what...`  the web server is running `laravel-admin` package which is vulnerable to `CVE-2023-24249` and there is a script that automates the exploit process and now we are the user `dash` and they have a `id_rsa` file in there `.ssh` folder in there home folder in the same home folder in the `.monitrc` file is some creds and the password works for the user `xander` `3nc0d3....` when we run `sudo -l` we can run a file called `usage_management` with no password to exploit this we need to make a file called `@id_rsa` in the `/var/www/html` folder and then sym link the file from root and then run the backup from the `usage_management` and it output the `id_rsa` key for the root user and we just edit in are notepad and then we can access the root user 