Started with a nmap scan
```
22 OpenSSH 8.9p1
80 Apache 2.4.52
```
There is a domain being redirected on the webhost `http://searcher.htb/` did a subdomain scan and did not find anything going to the website its running a `searchor 2.4.0` instance and there is a vulnerable python module that allows us to get RCE and we get a call back as the `svc` user once on the machine there is a `.git` folder with a config file that has a url to a `gittea` instance and it gives us the creds to the gitea `cody:..................` and we need to add `gitea.searcher.htb` to the host file and now we can log in and its running version `1.18.0+rc1` we can also use these creds to login over SSH as the `svc` user and when we do `sudo -l ` there is a script we can run and we can use this to find the admin password for the gitea account looking back at the scripts there is a full checkup command that we can use to get root account with a reverse shell and now we are root 