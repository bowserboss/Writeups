Started with a nmap scan
```
22 OpenSSH 8.2p1
5000 Werkzeug 0.16.1 Python 3.8.5
```
Going to the webpage its a kids hackers tools and the site uses msfvenom to generate the payloads there is a exploit for this `CVE-2020-7384` and we can make a apk file and we can upload the apk to the site under the android section and we get a call back as the `kid` user we can add a keys to the .ssh folder and get a real shell we can inject some stuff into a log file called hackers and get a shell as the user `pwn` and then we can run msfconsole as root and now we are root  