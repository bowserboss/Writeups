Started with nmap scan
```
22 OpenSSH 7.6p1
80 nginx 1.14.0
```
The nmap scan shows the webserver redirects to a website `http://horizontall.htb` going to the site and clicking around and non of the pages load anything using code beautifiers found a subdomain in the javascript code `http://api-prod.horizontall.htb` going to do a gobuster scan on this webhost when we go to `/admin` it takes us to the admin login page and googling the name its running a cms called `strapi` version `3.0.0-beta.17.4` and there is a exploit for this version `CVE-2019-19609` `CVE-2019-18818` and it will reset the admin account password and now we can login to the admin account  and we can sue the other one to get a reverse shell and we get a callback as the `strapi` user we can use PwnKit to get root or there is a Laravel framework running and we can exploit it to get root as well 