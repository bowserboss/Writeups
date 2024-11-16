Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.6p1 Ubuntu
23 telnet
80 Apache 2.4.29
61337 Werkzeug 2.0.1 Python 3.6.9
```
The ftp does not have anonymous login the telnet also needs a login 
Going to the webserver on port `80` if a default apache page could not find anything on this webserver 
Going to the webserver on port `61337` we get a login page doing a gobuster on this webhost 
```
/home
/login
/admin 403
/account
/external
/application 403
/application/console 403 probley python console 
/robots.txt
/internal
/temporary 403
/temporary/dev 403
/temporary/dev/newacc
AFTER LOGIN
/internal
/external
/account
/logout
```
On the 403 folders could not find a bypass there is also some type of blacklist running for the username and password  
Panel already has a `admin` account
Possible accounts
```
support@templeindustries.local
external@templeindustries.local
hiring@templeindustries.local
internal@templeindustries.local
external@templeindustries.local
```
After we make a account on the dev link we can login but there is no functionality cant find anything so doing injections on the username we got SSTI to work with this payload `{{7*7}}` we got `49` back when we look at the account page  so it is vulnerable to SSTI and its the jinja2 template based on this payload `{{ "jinja2"|length }}` I got back under the username on account page the number `6` so now need to test variables since I do know there is some blacklist on the account creation page 
I was able to bypass the WAF with hex encoding  and was able to drop a file onto the server and get a call back as `bill` found the creds for a database `temple_user:4$pCM!&bEEs$SR8H` found we can edit the `logstash-sample.conf` file and `logstash` is running as root and this config reloads every 3 seconds so we can use this to run commands as root you can check this with `pipelines.yml` file and if you go to hacktricks there is a page on how to exploit this and we can put a reverse shell in the config and now we are root 