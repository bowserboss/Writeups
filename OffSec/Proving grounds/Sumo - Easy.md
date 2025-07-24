Started with a nmap scan
```
22 OpenSSH 5.9p1
80 Apache 2.2.22
```
Going to the webserver we cant find nothing but a `index.html` so running nikto says it maybe vulnerable to `shellshock` so testing the exploit for exploit DB and running it in burp and can get a reverse shell using the user agent and now we are `www-data` running linpeas shows its vulnerable to `dirtycow` so we can get the exploit for it on `exploit-DB` and compile it and now we are root user  