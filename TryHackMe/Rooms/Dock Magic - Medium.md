Started with a nmap scan
```
22 OpenSSH 8.4p1 Debian
80 nginx 1.18.0 ubuntu
```
From the nmap scan there is a domain being used `site.empman.thm` going to start with a subdomain scan and found some more 
```
backup
site
```
On the backup subdomain its a file listing and there is a zip file called `ImageMagic.zip` the zip file is the source code of the program and its version `7` not much we can do with this at the moment so going back to the site there is a login page and a sign up page and it ask us to upload a png file as a avatar maybe we can use the known CVE for image magic to read files on the host its `CVE-2022-44268` when doing this exploit don't convert the image just upload it and then run the identify command and we convert it from HEX and we can read the `/etc/passwd` file and we have a user `emp` and we can get the `id_rsa` key for the user and now we are `emp` user so we are in a docker container and when we check the cron tabs there is a cron as root but the libraries that the script call are not there and we have access to `/dev/shm` so we can make a file called `cbackup.py` and put a reverse shell in it and wait for it to be called by the job and now we are root in the docker now to get root to the main host we do the release agent 2 docker breakout and we can set up a reverse shell and now we are root and the last flag is in the home folder of `vagrant` 