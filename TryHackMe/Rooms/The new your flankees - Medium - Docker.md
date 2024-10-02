Started with a nmap scan
```
22 OpenSSH 8.2p1 Ubuntu
8080 Octoshape P2P streaming web service
```
Looking at the website there is a couple pages and on the debug page there is a little section in the code that has a `/api/debug/` and then gives us a key when we go to the path with the key in the url we get a custom auth success if w3e take a letter out we get a decryption error doing some google searching there is a exploit for the padding in oracle and the todo notes had said something about this also I came across a program called `padre` on github and if we run it it will decode the key that we have 
`padre -u http://10.10.153.179:8080/api/debug/$ -e lhex -err "Decryption error" "key"` 
and now we have what looks like a username and password 
`stefan1197:ebb2B76@62#f??...........................` 
and now we can log into the admin panel and we can go to a debug page and it ask us for a command when I do a command it gives us a message of `OK` so we can make a script with a reverse shell in it and then we have to chmod it and run it with bash and now we got a call back as root but we are in a docker container time to breakout of the container I downloaded linpeas and deepce and ran them both and I found we can start are own docker image so if we run this command we can breakout and get to the main host 
`docker run -it -v /:/host (container ID) chroot /host/ bash` 
and now we can get the final flag 