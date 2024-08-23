Started with a nmap scan
```
80 Apache 2.4.29
65000 Apache 2.4.29
```
Doing a gobuster scan on the webserver on port `65000`
```
/assets
/api
/api/upload
/grid
/uploads.php
```
Looking at the webserver the title to the page is Light Cycle and in the gobuster scan there is a hidden webpage called `uploads.php` that takes a POST request for only pictures and uploads it to a folder called `grid` when trying to upload something need to get all the request in burp and drop the `filter.js` file and accept all of the rest if you don't you need to restart the box it wont work if you upload a php rev shell and just change the file name to something like `test.jpg.php` it will upload go to `grid` and now we have a call back looking in the website configs I found a login for the mysql server `tron:IFightForTheUSers` and we have access to the DB `tron` and if you login to the mysql server we can find the creds for `flynn` the user on the host with a encrypted password its encrypted with MD5 and I cracked it pretty fast `@comp...` and now we can su into that user and if we run the groups command we have access to the lxd group and we can exploit this and once you do now you have root