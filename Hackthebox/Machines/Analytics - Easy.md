Started with a nmap scan
```
22 OpenSSH 8.9p1
80 nginx 1.18.0
```
In the nmap scan it showed there is a domain for the website `analytical.htb` there is one subdomain for the site `data` and its a Metabase login the main site is pretty much a static site besides the login page that goes to the metabase login the version of metabase is `0.46.6` which is vulnerable to `CVE-2023-38646` but to use this we need the token if we go to this path we can get the setup token `/api/session/properties` and there is a script on exploit db we can use to get a revshell and now we are the user `metabase` now if we look at the environment variables it has the login for the SSH user account `metalytics:,,,,,,,,,,,,,,,,,,,` when I ran linpeas it showed its vuln to the game overly exploit and we can use this to get root 