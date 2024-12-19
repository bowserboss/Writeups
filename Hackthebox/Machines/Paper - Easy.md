Started with a nmap scan
```
22 OpenSSH 8.0
80 Apache 2.4.37 centos
443 Apache 2.4.37 centos
```
Going to the webserver we get a default apache page so I'm going to start a gobuster scan did not find anything so doing a curl on the site and looking at the headers there is a domain we found `X-backend-server: office.paper` we add the domain to are host file and now we have a wordpress site on port `80` and default page of port `443` and its a wordpress site so going to run a wpscan looking at the version of wordpress its `5.2.3` and there is a exploit in it if we add `?static=1` it will leak some info about private post on the site and reading the post there is a another subdomain we need to add and it goes to a secret registration page we can make a account and  looking around the site there is a group chat  we cab get path traversal with `list ../../..` and we find `.env` file in hubot and it has a password of `Queenofblad3s!23` and we can SSH into the `dwight` user running linpeas shows us its vulnerable to `cve-2021-3560` and now we are root 