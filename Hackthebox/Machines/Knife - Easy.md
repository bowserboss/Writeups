Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
```
When going to the webpage it looks like a static page has no links so im starting a gobuster scan
```
/index.php
```
could not find anything so I started looking at the headers of the request and its running php version `8.1.0-dev` which has a backdoor in it with the user agent and we can exacute code with this and its running as the user `james` we can get a reverse shell and then when we do `sudo -l` we can run `/usr/bin/knife` as root and if we look at gtfoBins there is a exploit for it so we can breakout and get root `sudo /usr/bin/knife exec -E "exec '/bin/bash'"` and now we are root  