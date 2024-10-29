Started with a nmap scan
```
22 OpenSSH 7.6p1
80 nginx 1.14.0
3000 Node.js 
8989
```
Going to both sites they seem like there the same and when I did the nmap scan it said there is a `/admin` section that is a login page and on the index page it says its built with NoSQL and registration is closed for now going to start a gobuster scan and see if there is anything else on the webserver 
```
/images
/admin
/robots.txt
```
Going back to the login page on port `3000` in the source code there is a javascript code that shows us how to setup the login process in json so I changed my request to match it in burp and put the username as `dave` and with the password filed I added a `$ne` with a empty password and got a redirect with a jwt token and then it goes back to the login page decoding the jwt token we get the first flag and the flag is also the password for daves account and now we at the admin page and we can exacute code with this page and using a node.js reverse shell and we get a call back as dave checking around the file system a little bit found a file called `uid_checker` and this file we can also run as root with no passowrd running strings on it got us the 4th flag found the 3rd flag in the mongo database in daves-blog in table `whatcouldthisbes` now going back to the `uid_checker` file now to get to root there is a ROP exploit in the binary we found we can use ropstar and it made us a payload and then we run this `cat payload;cat | sudo ./uid_checker` and now we are root 