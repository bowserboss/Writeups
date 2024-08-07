Started with a nmap scan
```
22 OpenSSH 7.6p1
80 APache 2.4.29
```

Started a gobuster scan
```
/img
/css
/js
/fonts
/License.txt
/login.html
```
Did a nikto scan on the host just found some more webpages
Did a subdomain scan 
```
broadcast
```
There is a login page that seems not to work its also in `.html` and the subscribe to the newsletter does not seem to work as well just send a GET request 
The subdomain found has a working login page but we don't have creds going back to the website to the js files and there is a route that using a GET request and we can get LFI form it
`http://vulnnet.thm/index.php?referer=/etc/passwd` 
and we can now read files on the host and we have the other auth on the subdomain so now we can read files on the server we know that creds are often stored in the `.htpasswd` file so we can try and read the file and see what we get 
`http://vulnnet.thm/index.php?referer=/etc/apache2/.htpasswd` 
and we have a `developers` login with  a encrypted password using hash cat to decode it its encrypted in `apache MD5` now we have a login
`developers:9972.....ls` 
with this login we can now login into the broadcast part of the site and its a `clipbucket v4` did a google search and there is a couple exploits that can be done I got a shell on to the server and can read files now on this host there is 1 account called `server-management` also used the file upload vuln to upload a rev shell and now got access to the server and we are `www-data` looking in the webserver files in the includes folder there is a file called `dbconnect.php` and it has some mysql creds `admin:VulnN........` and looking in random folders I found a backup in the `/var/` folder and there is a folder called `ssh-backup.gz` I got this back onto my host and unzipped it and it has a `id_rsa` file so using ssh2john and then john to crack the file we got the password for the `id_rsa` password `oneT......now we have access to the `service` account
I ran linpeas and seen its vuln to PwnKit 