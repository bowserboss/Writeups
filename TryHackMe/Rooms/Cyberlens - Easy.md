Started with a nmap scan
```
80 Apache 2.4.57
135 Winodws RPC
139 Windows netbios-ssn
445 
3389 RDP
61777 Apache Tika 1.17 Server CVE-2018-1335
```

Started a gobuster scan with the `2.3-medmium` list
```
/images
/css
/js
```
Checking for subdomains also no subdomains were found

The site seems to be build around `html` pages with some `js` files looking at the source code on the main page found a `json` call to a port on the web server `61777` and goes to `/meta` and looking at the main page on the hidden port it gives us all the link formats to make calls to the site   
The `tika` server is vulnerable to `CVE-2018-1335` and we got shell access with the metasploit module for this cve with this we can get the user flag using metasploit we background the session and load the `local_exploit_suggester` and found a couple possible exploits I found its vuln to `cve-2021-40449` and now we got system and can get the admin flag