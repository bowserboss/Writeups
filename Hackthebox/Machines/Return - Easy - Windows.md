Started with a nmap scan
```
53 DNS
80 IIS 10.0
88 Kerberos
135 RPC
139
389 LDAP
3268 LDAP
5985 
```
And there is a domain `return.local` Going to the webpage its a admin panel for a printer we can see the settings and we get a possible username `svc-printer` and we can update a password but when we send a POST request all it updates in the hostname so if we start `responder` with `sudo responder -i tun0` and update the hostname to are ip we get the creds for the user back `svc-printer:1edFg43012 !!` and looking at what shares we have access to all of them and we can winrm as this user and get the user flag using metasploit's local exploit suggester we can use CVE `2021-40449` to get to admin user   