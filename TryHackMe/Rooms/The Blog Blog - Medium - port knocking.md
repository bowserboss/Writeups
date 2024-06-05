Started with a nmap scan
```
21 vsftpd 3.0.2
22 OpenSSH 6.6.1p1 Ubuntu
80 Apache 2.4.7 
445 Apache 2.4.7
8080 Werkzeug 1.0.1 Python 3.5.3
```
Started a nikto scan did not find anything
Started a gobuster scan did not find anything

On the main page for the site there is some base64 in the source code that converts to brainfuck and decodes to `when I was a kid my friends and I would always knock on 3 of out neighbors doors always houses 1 then 3 then 5`  So this leads me to think were going to be doing some port knocking with this command 
	`for port in $(sed 1 2 65000); do echo "Knocking on port $port"; nc -z -w1 <ip> $port;`
And now we can see three more ports `21,445,8080`

There is also at the bottom another note that says `bob` forgets his password a lot and its encoded with base58 `HcfP8J5...4` and decodes to `cUpC....` ftp pass for bob
On port `445` its the same default page on all the web servers but this one in the source code has another comment that bob cant remember his password so this one is `p@55....d` 
The other password we found earlier works for the ftp with the username bob the ftp has 2 files on it one called `cool.jpeg` and `examples.desktop` so looks like it might be someone home folder 
Looking at the jpeg file I used stegseek to crack the password for the jpeg it was the same password we found in the source code on port `445` looking at the out file looks like another login and possibly and web server directory
```
zcv:p1fd3v3amT@55n0pr
/bobs_safe_for_suff
```
This login we found is really encoded with `vigenere cipher` here is is decoded
	`bob:d1ff3r3ntP....0rd`
Using what the what we thought was a folder on a web server some were its a file on the web server on port `445` that says 
`you need to get into the blog this will be taken down tomarrow - youmayenter`
Doing a gobuster scan on the port `445` 
 ```
 /user is a private ssh key with no auth magic 
 /bobs_safe_for_suff
```
Doing a nikto scan on port `445` did not find anything

Doing gobuster scan on port `8080`
```
/blog
/login
/review
/blog1
/blog2
/blog3
/blog4
```
Now we can log into the blog with these creds `bob:d1ff3r3ntP@5....d` there not much going on with this site but a couple short blog post and a review section just randomly testing random commands and input I did `ls` command and went back to the review page and show me the out put of that directory using just a bash rev shell and then went back to the review page we got a call back now there are 2 users on the box `bob` and `bobloblaw` looking at the binary's we got permission for as www-data there is one called `blogFeedback` does not do much so we got the binary back onto my machine and used ghrida and found that it spawns a shell as bobloblaw using a certain order `./blogFeedback 6 5 4 3 2 1` and now we got a shell as bobloblaw then there a file that gets backed up in the documents folder and we replaced `.bording_file.c` with a reverse shell and got root