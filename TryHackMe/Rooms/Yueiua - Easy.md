Started with a nmap scan
```
22 OPensSSH 8.2p1 
80 Apache 2.4.41
```
Starting a gobuster scan
```
/assets
/assets/index.php
/about.html
/index.html
/contact.html
/courses.html
admissions.html
```
NIkto scan did not find anything 
SQLmap did not find anything on the contact page 

Was doing alot of directory searching and finally found something in the `assets` folder its a index.php file and we can use ffuf to see if it has any pramters and I found one `cmd` so we have code exaction on the server as `www-data` and when we get the output its `base64` encoded now were on the server and there is a folder called `hidden_content` and has a file called passphrase.txt that has a base64 that decodes `Allmight....!` and in the images folder on the webserver there is a pic and its courpted but if we change the hex on it we can fix the file and then use stegseek to see if there is anything in the picture stegseek did not crack anything but we did get the word from the file eailer that's the pass for the picture and we got creds now to the `deku` user `deku:One?For?All_........` now as this user we can run a script called `feedback.sh` but we can now modify the file and there is a blacklist of characters but we can use the `/ and >` so we can try to right to the `/etc/passwd` file and we got a user into that file 
`'bowser:hash' >> /etc/passwd` 