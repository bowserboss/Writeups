Started with a nmap scan
```
80 lightttpd 1.4.35
443 lightttpd 1.4.35
```
Going to the webserver it redirects to the `https` the login page says its sense going to start a gobuster scan
```
/index.php
/help.php
/themes
/stats.php
//edit.php
/license.php
/system.php
/changelog.txt
/exec.php
/wizard.php
/pkg.php
/xmlrpc.php
/system-users.txt
```
while scanning the webhost found a file called `system-users` and has a account login  in it `Rohit:pfsense` and now we can login into the web application and its running version `2.1.3` and it has a exploit `CVE-2014-4688` its a command injection vulnerability and there is a metasploit module for this exploit and we can use it to get root to the server  