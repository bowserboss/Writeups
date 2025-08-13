Started with a nmap scan
```
21 vsftpd 3.0.3
25022 OpenSSH 8.6
33414 Werkzueg 2.2.3 Python3
40080 Apache 2.4.53
```
Starting with the webserver on port `33414` since the FTP just times out when you try to list stuff on it going to the site its a `404` page so running a gobuster scan on it and we find a couple files
```
/help
/info
/file-list
/file-upload
```
going to the `/help` it gives us all the url we can go to on the host and one of them we can read files with that are on the host testing the file upload since we cant do much with the read atm tryed to upload a php file to the web root but it failed but we can upload files to `alfredo` home folder and he has a `.ssh` folder we can also write to so we can upload are own ssh key to it and now we have access to the server to get linpeas onto the host we need to also use the file upload we can see there is a script on a cron running very minute as root and looking at the script if we add are own tar file into the restapi folder and add some commends into it we can get it to exacute as root and get the flag or a shell 