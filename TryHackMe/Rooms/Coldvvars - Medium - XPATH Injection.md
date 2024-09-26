Started with a nmap scan
```
139 smaba
445 smaba
8080 apache 2.4.29 ubuntu
8082 node.js 
```
Looking at the smb client there is some shares the normal ones but one is not normal `SECURED` but we can not access it with the guest account when I ran enum4linux it did find two users `
```
ArthurMorgan 
marston    This usernmae a guest account
```
Running a gobuster on the port `8080`
```
/dev
/dev/note.txt   Secure File upload and Testing Functionality
```
Running a gobuster on the port `8082`
```
/login
/static
```
The login page for this app in vulnerable to XPATH injection when we go `"` we get internal server error and running this injection `" or true() or "` returns a list of usernames and passwords 
```
Tove:Jani
Godzilla:KONG.....
SuperMan:sny...
ArthurMorgan:Dea...
```
Arthur account works to login into the smb server its the only one and now lets login into the smb server and see what's in there with this command we can connect to it
`smbclient //10.10.36.96/SECURED -u ArthurMorgan -p Dea....` and there is one file `note.txt` so what we can try to do is upload a rev shell to this since we can view it in the web browser and we get a call back as `www-data` and we can go into Arthur home folder now that were in we can just su to Arthur with his password and now were him and we can get root with PwnKit or sudo barren