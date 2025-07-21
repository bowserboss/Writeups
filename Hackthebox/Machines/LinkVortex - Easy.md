Started with a nmap scan
```
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; 
80/tcp open  http    syn-ack ttl 63 Apache httpd
```
The nmap scan showed us a domain that that webserver is redirecting us to `http://linkvortex.htb/` going to do a gobuster scan there is a signup page but cant sign up 
```
/assets               (Status: 301) [Size: 179] [--> /assets/]
/LICENSE              (Status: 200) [Size: 1065]
/ghost/
```
doing a subdomain scan found one subdomain `dev` and when we go to the site it says it will be coming soon so lets to a gobuster on this site 
```
/.git/
```
Found a git on the dev site we can use git dumper to get the files looking at the git dump but we cant run any commands says the HEAD file is broken looking threw the git found a file called `authentication.txt.js` and it has a password in it `OctopiFociPilfer45` and the user account is `admin@linkvortex.htb` I was  searching around google and found a exploit for the `ghost cms` `CVE-2023-40028` and its vulnerable to it reading the `/etc/passwd` file found a user `node` looking around for config files and looking at the github to see the locations I found this file ` /var/lib/ghost/config.production.json` and it has some creds `bob@linkvortex.htb:fibber-talented-worth` and now we have access to bob account on SSH if we run `sudo -l` we can run this cleanup script and we can exploit this with symlinks if we run these commands we can get the `id_rsa` file 
```
ln -s test.png maggimm.png
ls -s /root/.ssh/id_rsa /tmp/maggimmm.png
export CHECK_CONTENT='/bin/cat /root/.ssh/id_rsa
sudo /usr/bin/bash /opt/ghost/clean_symlink.sh maggimm.png
```
and it will cat out the key file and now we can SSH as root 