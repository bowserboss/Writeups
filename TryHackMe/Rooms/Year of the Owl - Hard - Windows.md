Started with a nmap scan
```
80 Apache 2.4.46 PHP 7.4.10
139 netbios
161 UDP SNMP
443 Apache 2.4.46 PHP 7.4.10
445 ds
3306 MariaDB 10.3.24
3389 RDP
5985 HTTP API
47001 HTTP API
```
Going to the site both port `80` and `443` seem to be the same site going to run a gobuster on the site
```
/examples 503 error 
```
There is also smb running but we can access it its also running `windows 10` also did not find anything on the webserver and could not find anything so star ted back with some more nmap scans and ran a UDP scan and we can see there is a port `161` open which is SNMP but we need to get the community sting so we can access the SNMP so running a tool called `onesixtyone` we found it `onesixtyone 10.10.241.159 -c snmp-onesixtyone.txt` and now we have it `openview` we can use `snmp-walk` to enumerate it `snmp-walk 10.10.241.159 -d` and we get a bunch of information like users `Jarenth,Guest,Administrator` but we don't get much more then this so lets get to trying bruteforceing the smb or any of the services we can use netexec or hydra to do this and we get a login `Jareth:sarah` now checking with nxc to see if we have winrm access and we do so now we can use `evil-winrm` to access the machine now we need to get a better shell so we can make a payload with msfvenom and now download it to the host and run it and we got a metasploit session checking with metasploit about any exploits we may be able to use but non worked so going back to the host looking around there is antivirus running since my first payload keeps getting removed and had to make a better one so we cant use winPEAS or `powerup.ps1` since they get blocked looking around did not find anything we don't have any crazy permissions so maybe if we look into the recycle bin we can maybe recover some files that may have been deleted running this command we can get are SID `whoami /all | select-String - Pattern "jareth" -Context 2.0` and we have are SID now we can cd into the recycle bin and we have a `sam.bak,system.bak` files we can use these to crack accounts on the host we can use a Impacket script called `secretsdump` `impacket-secretsdump -sam sam.bak -system system.bak LOCAL` and now we have all the hashes for the accounts using the NTLM hash for admin we can `evil-winrm` into the account and get the admin flag 
  