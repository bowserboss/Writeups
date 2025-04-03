Started with a nmap scan
```
21 FTPD
22 OpenSSH 7.7 for windows
80 HTTPD IIS
1311 rxmon?
3389 RDP
```
Going to the FTP it has anonymous login enabled and there is two files 
```
Credit-Cards-we-Pwned.txt
stolen-passport.png
```
The credit card file is 100 credit cards and the png file is a passport next we will check out the rxmon which is a https website on port `1311` `dellemc openmanage` its running version `9.4.0.2` now going to check out the webserver looking at the site has a couple pages all static pages the contact forum just sends a get request when try to send anything so we have nothing but there was a exploit for the DELL system `CVE-2020-5377` there is a PoC on `exploit-db` but this one will not work since we need to string two exploit together but the people who published the exploit have a another one on there github and this one will work with and we can check to make sure the exploit is working by reading `C:\Windows\win.ini` after trying a bunch of files I tried the `web.config` file and got it to read back to me `C:\inetpub\wwwroot\hacksmarteresec\web.config` and we get Tyler creds testing RDP we could not connect but SSH worked we are dealing with AV since metasploit payloads are getting flagged and winpeas truing another script like winpeas called `privsecCheck.ps1` and we can run this script `. .\PrivsecCheck.ps1; Invoke-PrivescCheck.ps1 -Extended` running this script we can see we can exploit the `spoofer-schedler.exe` and we can just double check if we can make a file in the folder `C:\Program Files (x86)\Spoofer\` but if we make a msfvenom payload it will probley get detected the person who made this machine has made a revshell in `nim` we can compile it after we installed nim `nim c -d:mingw --app:gui rev_shell.nim` and this way of building it will have the least detections now we need to go back to cmd and run `sc stop spoofer-scheduler` and now we need to remove the file and then we need to download the new file and make sure its named the right name and then we can start the process agin `sc start spoofer-scheduler` and now we have `system` user
