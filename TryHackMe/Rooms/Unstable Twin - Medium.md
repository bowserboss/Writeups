Started with a nmap scan
```
22 OpenSSH 8.0
80 nginx 1.14.1
```
Started a gobuster scan on the host 
```
/info   ## Tells us the logon API needs username and password and may not be secure 
/api/login
```
While I was waiting for gobuster I started guess some folders and found the api and was able to send a post request with curl and when I had logging enabled it would say bad password or username and when trying basic sqli injections and got it to 500 error out so I got the request to burp and played around with it a bit then I ran sqlmap I was stuck at this point a little bit when I was looking back at the curl commands I was running to the host I noticed the `/info` page would return two different build numbers and server name we can make a python script to enumerate this and sometimes you will have to run it two times using this payload we can read a note with a password in it `1' UNION SELECT user_id, notes FROM notes -- -`  its hashed in `SHA-512` and cracked to `...........` we got the username from the user table eailer `mary_ann` and we can SSH into the host now  running `ps aux` I found were the web app is running and there some images in the webapp but we cant get to them so looking at both source codes we can curl the images down with this command
`curl 10.10.191.95/get_image?name=linda` 
do this for all the images in the both sources codes and then using steghide and can extract them since its all blank passwords and we need to add them in order of the colors of the rainbow and its a base62 encoding and we get the final flag 