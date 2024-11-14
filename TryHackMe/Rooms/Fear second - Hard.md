Started with a nmap scan
```
22 OpenSSH 8.2p1
8000 Werkzeug 2.0.3 Python 3.8.10
```
Going to the website we can make a account that needs to be more than 6 letters two things were going to try and test is sql injections in the login page and in the register page I tried to enumerate this with sqlmap but its not possible to enumerate this we going to use union injections but we have to make a account with the injection as the username and then login with that injection and then enter something into the text box and we get the output be careful doing the injection because you can mess-up the database this is my first injection to get the database name
`a'union select 1,database(),3,4-- -` 
so now once we go this right it wont reflect are username anymore now it says there is `1 word website` so its working now time for the second injection
`a' union select all 1,group_concat(table_name),3,4 from information_schema.tables where table_schema=database()-- -` and in the output we get `only 1 word users` so now we got the table name we need time for the 3rd injection
`a' and 1=2 union select 1,group_concat(username,0x3a,password),3,4 from users-- -` 
and now we have some creds `smokey:S....................` and we can use these creds to SSH into smokey looking at `/opt/app/app.py` and there is Mysql creds `smokey:..................` in the database `second_project` there is a login for `hazel:........................` and in the dev database there is a login for smokey with a hash at this point while im trying to crack the hash im running linpeas on the host found there is a webapp running as the user `hazel` on port `5000` I port forwarded to my host and its a login page we can try the creds we found and this is vulnerable to SSTI if we make a account with this payload as the username `{{7*7}}` and login we get 49 back so we got SSTI so looks like we have to do the same process as with the sql injection so there is a blacklist on this one of 
```
config
self
__
""
```
so we need a payload that does not use any of these we need to encode everything that is in the blacklist in hexadecimal to bypass it so the payload would look like this 
`{{request['application']['\x5f\x5fglobalst\x5f\x5f']['\x5f\x5fbuiltins\x5f\x5f'][\x5f\x5fimport\x5f\x5f]('os')['popen']('ls -la')['read']}}` and we can make a sh file and download it and then exacute it and we have a reverse shell as `hazel` now as hazel we can edit the site config for the site running on port `8080` and then edit the `/etc/hosts` file and change the `dec_site.thm` to are ip and there was a script running every minute but lets capture the traffic and see what we get if we stand up a python webserver and open wireshark so we can try and capture creds or something need to make sure to download the `index.html` file and we got a password from smokey and its for root `.......................` 