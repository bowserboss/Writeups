Started with a nmap scan
```
22 OpenSSH 7.6p1 Ubuntu
7777 Python 3.11.0 Werkzeug 2.3.4
```
Started a gobuster scan
```
/cloud
/debug
/debugresualt
```
The `/cloud` has a list of files we can download they seem like junk files and some files it list are not on the server for download 
The `/debug` page seems like it could be a login forum since when we give bad creds it says wrong password 
So going back to the cloud page if we send the request to burp and try to download some of the webserver files we know there is a max length of 6 and the first letter might be a `s` so I tried `source.py` and I got the file looking at the source code there are some more files that we can try to get like `supersecrettip.txt` if we try to download it with burp we get a xor key so lets try to get the other python files like `debugpassword.py` we have to use a null byte to download it and it gave us are key `ayham` and we cracked the XOR and the password is `AyhamD......` so if we go back to the debug page and enter this payload `{{7*7}}` with the password we got and then curl the `debugresualt` page with are session cookie and this header we found in the `ip.py` file
`X-Forwarded-For: 127.0.0.1` and we get the response of 49 back so we have ssti and we can get a reverse shell from this by hosting are own sh file and then have the server curl the download and run it and now we got a call back  as user `ayham` and we can edit the file `/home/F30s/.profile` and put a reverse shell in it and now we are user `F30s` and to get root wwe seen in the linpeas output that root as a cron job running and curling a config file from F30s home folder so we can upload a `/etc/passwd` with are own user to get root but the flag is encrypted and the secret.txt is also encrypted but there is a txt file in the `/` folder we can make a wordlist out of and then get the key to decode the data but its missing 2 numbers so we need to generate a new wordlist that goes from `0-99 ` and brute the key agin 