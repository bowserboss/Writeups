Started with a nmap scan
```
22 OpenSSH 8.9p1
80 nginx 1.18.0 (Ubuntu)
```
The nmap scan shows there is a domain linked to the site `http://strutted.htb/` 
Starting with a subdomain scan
Going to the site there is a download function which downloads a zip file that's looks like its part of the code for the site there is also a image upload on the site looking at the zip agin to see what the site is running its running `tomcat` and `java` and looking at the `pom.xml` were running `apache struts` version `6.3.0.1` and this site is vulnerable to `CVE-2024-53677` the script wont work so we have to do this manually and we have to add a second parameter called `top.UploadFileName` and send the name of the jsp file `.././w.jsp` and in the first parameter we need to fake a image header so it thinks we have a image and once uploaded we can go to the website and get are shell `http://strutted.htb/uploads/../../3.jsp` and now we need to make a reverse shell script and then put it in the `tmp` folder and exacute it and we get a call back as `tomcat`  and looking in the web config folder we find a file called `tomcat-users.xml` and it has a non standard password and it works for james `IT14d6SSP81k` and when we run `sudo -l` we can run tcpdump with no password as the root user and we can use this to get the root flag 