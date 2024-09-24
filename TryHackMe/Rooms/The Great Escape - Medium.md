Started with a nmap scan
```
22 ssh?
80 ningx
```
Starting a gobuster scan
```
/api
/robots.txt
/exif-util
/*.bak.txt$
```
Sign up is disabled since there is not much going on with the site in the robots file there is a exif tool section that can take a link from a url and we can use this to bruteforce the localhost ports we found a port `8080` and it says nothing to see here and this had called a new route `/ecif?url=` but this at the time did not lead to much and brute forceing the well known files and we found a `exif-util.bak.txt` and this leads us to another url in the code of the file `api-dev-backup:8080` and testing injections on this I found a command injection but its filtering words `echo <fdasfa;ls -la` and url encode it and now we can read the file system and we are in  a docker and when we go to root there is a file called `dev-note.txt` and it has a password in it `fluffy.........` there is also a `.git` folder and we can run this command so we can interact with the git repo `git --get-dir /root/.git/` and then we can end it with log and see all the commits and if we go to the first one and then use the diff command and then the hash we get a new note saying we need to port knock on these ports `42 1337 10420 6969 63000` to open the docker tcp port  after knocking on these ports with knock or telnet we can run  a port scan and found a new port called docker `2375` and we can list the images and get into the real root with this command `docker -H 10.10.162.97 run -v /:/mnt --rm -it frontend chroot /mnt sh` and now we are root and can get the web flag and the root flag 