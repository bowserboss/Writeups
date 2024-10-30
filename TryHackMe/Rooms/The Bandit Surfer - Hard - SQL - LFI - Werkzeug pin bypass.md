Started with a nmap scan
```
22 OpenSSH 8.2p1
8000 Werkzeug 3.0.0 Python 3.8.10
```
Going to the website there is not much going on with the web app but a download functionality and there is a sqli injection in the `id` parameter and we can now dump the database with sqlmap this did not do much for us so I started a gobuster scan and see what else is on the host
```
/download
/console
```
When we go to the `/console` page it ask us for a pin but not much we can do here going back to the sqli injection I found we can read files with this `' UNION SELECT "file:///etc/passwd" -- -` and now since we have this we can do the console pin bypass by following it on hacktricks and they have a script for it got the pin now `655-302-140` and we have access to console now and now we can just take a python reverse shell and we get a call back as `mcskidy` I put my own ssh pub key in the authorized keys file so I can get a real shell going to the webapp folder I seen a `.git` folder so I started looking at some of the old commits and found a old password turns out its the ssh password for `mcskidy:F45......` and now we can sudo and we can run `/opt/check.sh` file as root so if we make a file called `[` and put a reverse shell in it and then make it executable and then run the script as root we get a call back as root now  