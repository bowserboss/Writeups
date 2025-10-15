Started with a nmap scan
```
21 FTP
80 Indy http 18.1.37
135
139
445 smb
5985 winrm
```
We have anonymous FTP login and we can get the first flag in the Public Desktop folder Now looking at the website its a PRTG Network Monitor version `18.1.37` with has a authenticated RCE but we need creds first since the default creds don't work so  doing some google searching and found the location were the backup is stored `C:\ProgramData\Pasessler\PRTG Netowkr Monitor\PRTG Configuration.old.bak` and looking for anything with password gives us the creds in plain text `pprtgadmin:PrTg@dmin2018` but this is the wrong password from guessing a couple times by changing the year we got the new password they changed it to `2019` and now we can use that exploit to get SYSTEM shell 