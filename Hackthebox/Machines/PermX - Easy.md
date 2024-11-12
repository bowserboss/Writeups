Started with a nmap scan
```
22 OpenSSH 8.9p1
80 Apache 2.4.52
```
The webserver redirects to `http://permx.htb` 
Starting with a subdomain scan only found one `lms` 
Going to the subdomain its a login portal for a learning technologies there is a cve for this `CVE-2023-4220` and there is a poc online for it I got a reverse shell as `www-data` now I found creds to a ftp pass `gaufrette` also found a database password `03F6lY3uXAP2bkW8` user `chamilo` turns out the password for `mtz` is the same password as the database now we are user mtz when I do `sudo -l` we can run a file called `/opt/acl.sh` if we make a symlink to a file and then run the script we will get the contents of the file 
`ls -s /etc/passwd /home/mtz/fakefile`
then we run the script 
`sudo /opt/acl.sh mtz rw /home/mtz/fakefile` 
and then we echo a user into the passwd file and then do su root1 and were now root