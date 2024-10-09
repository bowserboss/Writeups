Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
8000 nginx 1.18.0
8080 Werkzeug 2.2.3 Python 3.8.10
```
Port `80` and `8000` when going to the site has a forbidden page port `8080` has a site we can view  doing a gobuster on port `80`
```

```
Doing a gobuster on port `8000` 
```
/robots.txt 
index.zip
```
The robots file has the first flag in it and also in the robots file there are 3 extensions `.sql, .bak, .zip` there is a `index.zip` file that has 2 files in it one is the second flag and there is a `app.py` file it has some paths we can get to on the `8080` webserver looks like when going to the site we have a older version of the `app.py` code so we can run a ffuf and scan for the new parameter for the `password_reset` page and we got the new one its called `token` and when we give it a time and date in the format we got the code we found it also has to be hashed in `sha1` and when we go to it we get a invalid token now and not invalid parameter now we need to make a script to bruteforce the usernames and get the password reset I had chatGPT help me out with this now we also need to make a list of usernames there is 3 possible usernames we got from the `app.py`  or we can just use a premade list from seclists now that we got the username `administrator` and bruteforce the hash and we can reset the admin account password and we get the 3rd flag after testing some stuff I tried to access the localhost on port `80` and we get action not permitted so maybe we have ssrf we can do `http://0:8080/` and we can access the webhost after doing some bruteforceing with burp were are able to get to that database file with this payload `http://clocky:80/database.sql` and we got the 4th flag and we have a hard coded password and after using the username list agin I found one user that could login into ssh with this password and its `clarice` password `Th1s_1s_4_v3...................` there is a `.env` file in the app folder and it has the passowrd for the database and we have the use the user `clocky_user` and we can see some users in the database and crack the mysql hashes there is the `mysql` database since hashcat cant handle these hashes there is a github talk on there repo on how to convert them to another hash and crack them with mode `7401` and we cracked a hash and we can now login as root with pass `............` 