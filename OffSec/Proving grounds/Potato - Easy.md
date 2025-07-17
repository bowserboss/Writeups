This machine gives us creds to something `webadmin:dragon` 

So going to start with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
2112 ProFTP
```
Going to start with the FTP since it has anonymous login and has 2 files on it 
```
index.php.bak
welcome.msg
```
On the `index.php.bak` file there is another login `admin:potato` and has a note to change the password regularly now going to the site it says its still under construction so going to run a gobuster scan on it 
```
/admin
/admin/logs/
/potato  ## just a open folder nothing in it
```
going to the `/admin` we get a login page but none of the logins we have work trying to get hydra to work and see if we can bruteforce the web login but no success so I tried other ways to bypass the login page and if we go to burp and add `[]` to the end of the password pram we get a successful login and we can view the admin pages and  not much we can do here we can see the users on the site which is `admin` we can ping Google's DNS server and view the log files we all ready had access to so I'm gonna try the creds that we have for SSH and they work now we have access to the machine I ran `sudo -l` and we can run the tool `nice` as sudo over the `/notes/*` folder and we can exploit this by making a sh script in are home folder and put this in there `/bin/bash -p` and the call the script with sudo `sudo /bin/nice /notes/../home/webadmin/bash.sh` and now we are the root user 