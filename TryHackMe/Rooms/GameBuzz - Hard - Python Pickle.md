Started with a nmap scan
```
80 Apache 2.4.29
```
Going to start with a gobuster scan
```
/static
/fetch 405
```
Looking around at the site its a pretty static site the only functional parts about the site is the contact forum the newsletter section and the game ratings running a sqlmap scan on the contact forum did not find anything looking at the game ratings its a python pickle so we can try a deserialization attack on this but we need a place to upload files so going to start a subdomain scan and take a guess that the domain linked to the site is the same as the contact forum the only I found was `dev` going to the site its just a page that says its for devs only going to run a gobuster on this 
```
/secret
/secret/upload/
```
When we go to the upload one now we have a file upload when can use  a python3 pickle script upload it as `test` and then go to folder `/var/uploads/test` in the burp request and we get a call back as the `www-data` user looking around the file system in the mail folder there is a message for `dev1` saying password has changed and has a MD5 hash that cracks to `Passowrd` but cant su into `dev1` but when I tried `dev2` we got in with no passowrd going to the home folder we get the user flag so now ima drop linpeas onto the system and scan it we found some `knockd` files looking at the config if we knock on these ports `5020,6120,7340` and now SSH is open now we were never supposed to crack that hash I think because if we use the hash we can log into the `dev1` and when we run `sudo -l` we have root permissions over the knockd service we can edit the config file to put bash into the tmp folder and then restart the service and then run `./bash -p` and now we are root 