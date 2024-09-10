Started with a nmap scan
```
25   smtp hMailSevrer
80   Microsoft IIS 10.0
110 hMailServer pop3d
135 RPC
139 netbios-ssn
143 imapd
445 
465 smtp
587 smtp
993 imapd
5985 HTTP API 2.0
7680
49664
49665
49666
49668
63953
```
Checking out the website there is a domain that goes with it `mailing.htb` looking around the site not a whole lot going on with it but there is a download function of the site that has a LFI in it and we can read the config file for hMailServer `http://mailing.htb/download.php?file=..\..\..\ProgramFiles+(x86)\hMailServer\Bin\hMailServer.ini` and we got a password that's encrypted I got it decrypted `homenetworkingadminstrator` and looking more at the mail server there is a CVE for it `CVE-2024-21413` now that we have a password we can use this and we can get usernames from the site there are 3 people listed we need to put up a smb server to capture when we send a email to each team member we got the hash now for `maya` and we can crack it `m4y4.....` now we can use `evil-winrm` to access the PC there is `LibreOffice` on the box and its version `7.4.0.1` and has a RCE exploit out `CVE-2023-2255` and can make a revshell exploit with this and we also need to upload nc.exe to the program data folder so we can get a call back and then we upload the exploit to the `c:\Important Documents` and then wait for the call back as `localadmin`  