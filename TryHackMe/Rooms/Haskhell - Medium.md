Started with a nmap scan 
```
22 OpenSSH 7.6p1  Ubuntu
5001  Gunicorn 19.7.1
```
Started a gobuster 
```
/submit
/uploads/
```
Started a nikto scan

The web app only takes Haskell files `hs or hsl` are the file exertions and then uploads it to 
	`/uploads/` folder
Still looking threw the site I found were to submit the files and it tells us that it will compile are code for us and had ChatGPT write me a script to read `/etc/passwd` file and uploaded it and it read out the passwd file now time for reverse shell using this script it took be a bit trying a bunch of different ones
```
import System.Cmd

main = system "reverse shell"
```
Now we got a shell I use ones of the users has a ssh folder and get there `id_rsa ` and now we got ssh access and user flag and to get root used pwnkit