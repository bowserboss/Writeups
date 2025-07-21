Started with a nmap scan
```
22 OpenSSH 9.6p1 (Ubuntu)
80 nginx 1.24.0
```
The nmap scan shows a redirect to a domain `http://cypher.htb/` looking at the login page when I do `'` we get a python error and leaks the python version `3.9` web app path `/app/app.py` the hash is also going to be in `SHA1` and this is also running  `neo4j` and using a payload we can get the version of `neo4j 2.24.1` after testing so more injections we found `name` is the valid property and got a user `graphasm` and then got the hash but we cant seem to crack it so I went to see if we can get a reverse shell and we got one and now we are the `neo4j` user running linpeas now we found a file for the neo4j `.bash_history` file and has a password for the user we found `cU4btyib.20xtCMCXkBmerhk` now when we run `sudo -l` we can run `bbot` with no password looking at the docs we can run with `-cy` and it will read the file 
`sudo /usr/local/bin/bbot -cy /root/root.txt -d --dry-run` 