Started with a nmap scan 
```
21 vsftpd 3.0.3
80 Apache 2.4.18
443 Apache 2.4.18
18002 Java RMI
41743 Java RMI
44559 
```

Starting with the FTP server it allows anonymous login cant list if there is any files in the ftp

Starting a gobuster scan 
```
/docs
/themes
/admin
/assets
/upload
/tests
/plugins
/appliction
/tmp
/framework
/locale
/installer
/third_party
``` 
For the Survey site the login is `admin:pas...rd` there is only one user and its this one from looking at the sql db
Use `cve-2018-17057` we can get a reverse shell looking in the config file we found some creds for the db 
`anny:P4$W0....CUr3!` 
login for wordpress db `wordpressuser:passwo...` 
On the port `443` is hosting the wordpress site but it using WPS Hide Login to hide the login page metasploit there have a plugin that will find the hidden url for the login page using the login we found looking in the config files now we got access to `wp-admin` and now to get a reverse shell we need to make a php file with reverse shell php pentest monkey is good and then zip it into a zip file and upload it under plugins and activate it and now we got `www-data` 
On the server there is one user `veronica` and got root using pwnkit