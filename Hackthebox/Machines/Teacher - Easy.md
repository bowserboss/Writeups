Started with a nmap scan
```
PORT   STATE SERVICE REASON         VERSION
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.25 ((Debian))
|_http-title: Blackhat highschool
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
|_http-server-header: Apache/2.4.25 (Debian)
```
So all we have is a website and its a static website doing a gobuster scan on the webhost
```
/images               (Status: 301) [Size: 313] [--> http://10.10.10.153/images/]
/css                  (Status: 301) [Size: 310] [--> http://10.10.10.153/css/]
/manual               (Status: 301) [Size: 313] [--> http://10.10.10.153/manual/]
/js                   (Status: 301) [Size: 309] [--> http://10.10.10.153/js/]
/javascript           (Status: 301) [Size: 317] [--> http://10.10.10.153/javascript/]
/fonts                (Status: 301) [Size: 312] [--> http://10.10.10.153/fonts/]
/phpmyadmin           (Status: 403) [Size: 277]
/moodle               (Status: 301) [Size: 313] [--> http://10.10.10.153/moodle/]
/server-status        (Status: 403) [Size: 277]
```
we find a `/moodle` and it redirects a domain `http://teacher.htb/`  and there is a course and a login page taking a look back at what else we got there is a lot of images in `/images` and there is a file called `5.png` which is not a picture but a txt file so we download it and read the note and it gives us a username which we could match to the name of the course admin `Giovanni` and his password but its missing a letter `Th4C00lTheacha` and we got it cracked `Giovanni:Th4C00lTheacha#` and its vulnerable to `CVE-2018-1133` using this exploit we now get a call back as the user `www-data` looking in the `config.php` file we get the creds for the database `root:Welkom1!` now we can log into the database and use database `moodle` and the table is `msdl_user` and there are 4 hashes one if different from the rest we can try cracking them and one cracked to `giovanni:expelled` and now we are this user looking in the home folder there is a backup file and its recent if we run `pspy` also we can see a bash script that root is running `/usr/bin/backup.sh` but we can chnage the permissions if we make a symlink from `/work/tmp` folder `ln -s /usr/bin/backup.sh` and then we can add are reverse shell in there now and we get a call back as the root user   