Started a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 8.2p1
23 Telnet
80 Apache 2.4.41
8080 Werkzeug/2.2.2 Python/3.8.10
```

FTP does not have anonymous login
The telnet has a login for a `ubuntu 20.04.5 LTS` workshop login
The website on port `80` says its under construction 
Doing a nikto scan on this web host did not find anything
Doing a gobuster scan on this `80` webhost did not find anything
The website on port `8080` has a interact login
```
/login
/home
/admin
/external
/sms
/logout
/application
/internal
/temporary
```
Possible usernames
```
anders
devops
devops@securesolacoders.no
answers@securesolacoders.no
securesolacoders
```
Since I could not find anything else I had a wordlist generated and started bruteforceing the login page with the email that we seen in the code of the login page and the username is `anders:se.......2` after we login we see a SMS page we could try and see if we can bruteforce the SMS message by just keep trying and if we don't get blocked then we can so I made a list of numbers from 0000-9999 and the code is `....` 
`hydra -u http://10.10.64.217:8080/sms -w numbs -H 'Content-Type: application/x-www-form-urlencoded' -b $'session=(cookie)' -X POST -d "sms=FUZZ" -ac` 
Now we can see dashboard the only function on the pages are the `news=` pram that sends a post request and its vulnerable to LFI and using this file we could tell were the webapp was at `../../../proc/self/environ` so its in the home folder of devops now we need to guess the folder name and its not in a folder there is a `app.py` right in the home directory so were trying to get into the `/admin` section of the webapp we cant login but since we can read the app.py we can see the session cookie is weak so maybe we can bruteforce and sign are own cookie we can use `flask-unsign` to brute and then make a new cookie now we can access the `/admin` section and from the code that we seen eailer if we send a POST request with a `debug=` but does not reflect the commands so I put a revshell and url encoded it and got a call back  looking at the processes the website on port 80 that's running under the anders user and we can put files in the web root so we can put a shell in there and get a call back as the next user running sudo -l we can run apache2 as root and reboot it so lets looks to see if there any apache2 files we can modify found a file called `envvars` and we could make a bash binary and run it and then we are root