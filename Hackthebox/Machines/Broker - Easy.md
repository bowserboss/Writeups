Started with a nmap scan 
```
22 OpenSSH 8.9p1
80 nginx 1.18.0
1883 mqtt
5672 amqp
```
When going to the web server it says we have the login using the login `admin:admin` we got access to the `activemq` service using version `5.15.15` its vulnerable to `cve_2023_46604` and metasploit has a module we can use to exploit this or there is a good poc on github https://github.com/evkl1d/CVE-2023-46604 
now we have a shell as `activemq` and we can now get the user flag time for root flag now when we ran `sudo -l` we can run nginx as root and looking at this exploit https://gist.github.com/DylanGrl/ab497e2f01c7d672a80ab9561a903406 I just modified it a little bit and ran the commands and now we can curl the localhost to get the root flag 
