Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
```
Going to the site its a connection test the first thing I think off is command injection so we need to open ciado and intercept the request so we can mess with it testing it out and we get a ping back to are host so then I send a seconded request but this time I added `;id` and got `www-data` back in the request so now we need to get a reverse shell and we can do so with python3 and now we have a shell as `www-data` we can read the first flag in the `dylan` home folder going to run linpeas now and we have SUID bit set on `vim.basic` we can use this to get to root if we go to GTFObins and use the vim breakout we have to use the python3 version of it and we got root 