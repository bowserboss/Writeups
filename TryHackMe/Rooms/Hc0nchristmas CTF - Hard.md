Started with a nmap scan
```
22 OpenSSH 7.2p2
80 Apache 2.4.18
8080 webserver?
```
Going to the webhost on port `8080` its a hash that looks like base64 but did not decode right
Starting a gobuster on port `80`
```
/hide-folder
/hide-folder/1
/hide-folder/2
/images
/index.php
/login.php
/register.php
/admin  ## goes to a apk file
/robots.txt
/classes
/javascript
```
When going to the number `2` folder its a binary and when we run strings  on it when get a username and password and when we run the executable and when we give it the creds it says its part 2 of the SSH password and then going to folder `1` and send a request with the options method and get the first part of the ssh password  looking back at the webserver there is a cookie that gets set when we make a account and we can use padbuster to decode the cookie looking at the `robots.txt` file it gives us a user so we can impersonate this user with padbuster and in the `admin` folder there is a `apk` file that gives us a hint of the type of encoding the site uses `AES/CBC/PKCS5PADDING` and looking bad at the robots.txt file there is a `iv.png` we downloaded the picture and we have a hint about the `3031 grpup` there is a way to decode this picture and its `THEIVFORINGROAEY` now we can go back to the hash on port `8080` and decode it and we get the SSH user `thedarkangent` and with the passowrd for SSH we got eailer `,,,,,,,,,,,,,,,,,,,,,,,` and we got ssh access to get root we can exploit the binary in are home folder since it calls `/bin/sh` and runs as root 