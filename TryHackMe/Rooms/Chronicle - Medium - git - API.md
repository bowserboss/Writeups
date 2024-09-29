Started with a nmap scan
```
22 OpenSSH 7.6p1 Ubuntu
80 Apache 2.4.29
8081 Werkzeug 1.0.1
```
I went to the webhost and its a blank page that just says old so I started a gobuster scan
```
/index.html
/old
/old/.git/
/old/templates/
/old/templates/index.html
/old/templates/login.html
/old/templates/forgot.html
```
when going to `/old/` there is a note that says that everything has been moved and the webapp has been deployed and there is another folder called `templates` that's were the webapp is at so after a lot of enumeration of the webhost I finally found something a `.git` folder in the `/old` folder so lets use git-dumper to grab it off the webhost doing `git log -p` will show us all the commits and there is a key in one of them now time to move to the other webhost and see what we can find only the login page works going to also do a gobuster on this host 
```
/login
/api/
/forgot
```
Looking at the login page we still cant do much the password reset does not work either so looking back at the code and knowing there is a working api and we have the key `7454c262d....................` lets try and FUZZ for usernames found a usernames
```
tommy
someone
```
When we hit the api with the user `tommy` we get his login creds and these work for SSH `tommy:.....................` looking around the file system there is a `.mozilla` folder in the `carlJ` home folder so I zipped it and transferred it to my machine and lets see if there is any creds in it there is two profiles we need to decrypt the seconded one and we have to guess the password after making a couple guesses I did `pas.......` and got the creds now for `carlJ:Pas$w0........` and we are now carl we can do a buffer overflow for the smail binary but we can also just run PwnKit 