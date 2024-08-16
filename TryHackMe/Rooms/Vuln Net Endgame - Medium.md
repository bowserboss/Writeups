Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
```
Started a gobuster scan
```
/images
/css
/js
/fonts

admin1   subdomain
/en
/vendor
/fileadmin
/typo3temp
/typo3
/typo3conf
/type3/sysext

shop     subdomain
/images
/icon
/css
/js
/fonts
```
Doing a scan for subdomains
```
api
shop
blog
admin1
```
The `shop` and `blog` subdomain is the only two that load a site 
Did not find anything on the `api` subdomain but if you look at the `blog` you can find the path for the api
On the `admin1` subdomain its running `typo3 CMS` 
After looking for a while in the admin1 subdomain I went back to the ones that were giving me data and acting as sites went to the `blog` and there is a sql injection un this url
`http://api.vulnnet.thm/vn_internals/api/v2/fetch/?blog=1`
There is two databases that are not default `blog` and `vn_admin` looking in the database of `vn_admin` we find a username and a password hash we going to use john and see if we can crack the hash its `argon2` I tried using the list `rockyou.txt` but did not get anything after running it for a while but in the `blog` database there a bunch of usernames and passwords maybe we can try that list of passwords and it worked here is the password `vAxW....` and that login works for the admin panel now we can upload files right to the web server but its blocking some extensions so we have to go to the admin settings and configure wide and go to backend and there is a file deny list take out the php 3-8 and now we can upload php files get a rev shell in php and upload it to the file upload and then click on extended view and it will give us a easy link to go to and now we got a call back as `www-data` looking around the system could not find anything buy I did find a `.mozilla` folder and doing some google there is a tool called `firefox_decrypt` so we can just zip up the Mozilla folder and transfer it to are machine and then using this command we can decrypt the profile were the logins are stored 
`python3 firefox_decrypt.py /home/bowser/....../Vulnnet-Endgame/home/system/.mozilla/firefox/2fjnrwth.defualt-relase` 
and we get another login for his THM account and this password is the pass for the `system` account on the machine and we can ssh into it now in the home folder there is 3 binary's in the `Utils` on GTFObins the `openssl` this has a file write we can use and over write the `passwd` file we can gen a password hash with openssl on are machine and make a back up passwd file and then edit are login into the file and reupload it to the tmp folder 

Possible usernames
```
chris_w
system
```
