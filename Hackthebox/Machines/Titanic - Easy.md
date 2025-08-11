Started with a nmap scan
```
22 OpenSSH 8.9p1
80 Apache 2.4.52
```
The nmap scan shows a redirect to a domain `http://titanic.htb/` scanning for subdomains found one subdomain `dev` going to the main site it does not have much besides a book now button and when you submit something there it downloads a json file of what you entered and comes from `/download?ticket=` going to run a gobuster scan and see if there is anything else
```
/download
/book
```
Going to the `dev` site its a gitea instance looking at the repos there is one that has the source code for the site and there is a docker repo as well and in the `docker-compose.yml` file there is a password for mysql `MySQLP@$$w0rd!` the gitea is running version `1.22.1` looking around I cant find anything cant make a account get `500` error so ima go back to the main site and test that only function we can find need to test the download for url for LFI and we have LFI `/download?ticket=../../../../etc/passwd` and it downloads the passwd file and as we know from the code its running as the user `developer` user and we can get the flag we know the user for sure now looking around with the LFI and not finding much gonna try and crack the ssh login for the dev account and it cracked to `25282528`  or we can use the LFI to download the gitea database and crack the hash now running linpeas on the host there is image magick installed and there is a exploit for this version `7.1.1-35` `CVE-2024-41817` and we can use this to read the root flag or get root user there is also a script we found `/opt/scripts/identify_images.sh` that uses magick as root 