Started with a nmap scan
```
PORT   STATE SERVICE REASON         VERSION
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.41
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
|_http-favicon: Unknown favicon MD5: FED84E16B6CCFE88EE7FFAAE5DFEFD34
|_http-title: FlexStart Bootstrap Template - Index
```
Going to the website its a pretty static site there is a contact page but when using it says `PHP Email From` Library so going to do a subdomain scan
```
monitor
monitoring
zabbix
```
All the subdomains redirect to the same page a `zabbix` instance and we can use `IPMI` to dump hashes metasploit has a module for this `auxiliary/scanner/ipmi/ipmi_dumphashes` and set the information and now we have the hash for the `Administrator` and we can see this hash to hashcat and it cracked to `ilovepumkinpie1` now we can login into the zabbix instance and we can abuse this agent thats running if we go to the `monitoring` tab and click on `hosts` and then click on `items` and make a new item and set the `key` to `system.run[id]` and click test at the bottom and  in the value field we get `zabbix` as the user we can get a reverse shell now but after 5 seconds it drops the connection looking at the documentation of the `system.run` if we add `nowait` into the command the shell will stay now we are the `zabbix` user running linpeas did not see much but the user on the machine shares the same password as the admin zabbix user `ipmi-svc:ilovepumpkinpie1` re running linpeas we got the creds for the database now `zabbix:blooarskyblah` we can dump three users hashes but only one cracked to nothing the `mariaDB` is running version `10.3.25` and there is a OS command execution vulnerability `CVE-2021-27928` and we can use this to get a reverse shell as the root user we make a payload in a `.so` file with msfvenom then put the file onto the machine and start are listener and then login to mysql `mysql -u zabbix -pblooarskyslah -e 'SET GLOBAL wsrep_provider="/tmp/exploit.so"'` and now we are root   