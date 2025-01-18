Started with a nmap scan
```
22 OpenSSH 8.91
80 nginx 1.18.0 (Ubuntu)
8080 http?
```
Clicking around the site on port `80` its a pretty static site 
Running a gobuster on port `8080` when going to the site get a `404 error`
```
/website
/console
```
After bruteforceing folders for a while I trying making a wordlist from words on the site and we got a hit on port `8080` as `silverpeas` which takes us to a login forum  doing some google search found a CVE for a auth bypass as super user on the panel `CVE-2024-36042` and now we are admin 
with this auth exploit we can also get to the other users when looking at the scriptkiddy user we found he has a notification and it has a message id when you read the message we can try for idor here when testing random numbers we found a idor and found a SSH login for a user `tim`   
Possible Usernames 
```
scr1ptkiddy
```
When running linpeas we found a auth log we can read and there is a DB password in the log and its re used for the user `tyler` `_......` and when we run `sudo -l` we can just sudo into the root user with no password `sudo -u root bash` 