Started with a nmap scan
```
22 OpenSSH 9.1 Ubuntu
80 Apache 2.4.58
```
There is a domain for the webhost `instant.htb` 
Starting with a ffuf scan for subdomains did not find anything
Starting a gobuster scan medium
```
/css
/dwonloads/
/img
/js
/javascript
/index.html
```
Looking at the source code of the site I found a apk file used apktools to dump the apk and found some subdomains in it `swagger-ui` and `mywalletv1`  In the swagger it has some api links we can hit and we can make a account and and these api routes go for the other subdomain looking back at the apk file I found a admin jwt login `/smali/com/instant/AdminActivites` now looking back the swagger docs we can use this link to get the `id_rsa` key useing a LFI 
`curl -X GET "http://swagger-ui.instant.htb/api/v1/admin/read/log?log_file_name=..%2Fid_rsa" -H "accept: applicatoin/json" -H "Authorization: (admin jwt)"`
the username for the key is `shirohige` and now we have access to the ssh account looking around I found a `instant.db` file and to crack these hashes with hashcat need to take out the pbkdf2 and then separate everything with `:` and then need to base64 encoded the salt and hash after a little bit found the password for root `12**24nzC!r0c%q12` 