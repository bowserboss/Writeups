Started with a nmap scan
```
22 OpenSSH 8.9p1
80 nginx 1.18.0
```
When going to the site there is a domain linked to it `Heal.htb` and the login goes to a `api.heal.htb` so going to do a subdomain scan and see if there is anymore we can make a account and found another subdomain for the survey and its running `limesurvey` going back to the api there is a LFI we can use 
But need to bypass the token to get the token we need to log out of the account we made and then load up burpsuite and login back in and look at all the request and we will get a auth token and then we can copy this into are other request with the LFI looking at the docs we can find some paths we might be able to access 
```
- /download?filename=../../config/database.yml
    Reveals database credentials.
- /download?filename=../../storage/development.sqlite3  
    Dumps the SQLite database.
```
after we download the database we can get the hash for the user `ralph` with this command `sqlite3 development.sqlite3 .dump` and we have the hash now we can try cracking it cracked to `147258369` and now if we go to the survey site there is a `/index.php/admin` login page and we can use these creds to login in doing some google searching I found a CVE for a auth RCE in the lime survey `CVE-2021-44967` and we can upload a plugin for a reverse shell and now we get a callback as `www-data` looking in the config files we find a database password `AdminDi0_pA$$w0rd` and this also works for the user ron going to run linpeas now and see what else is on this machine and on port `8500` there is a service called consul and for this version there is a RCE exploit and we can use this to get a reverse shell as root 