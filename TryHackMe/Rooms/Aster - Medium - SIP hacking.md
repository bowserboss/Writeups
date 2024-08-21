Starting with a nmap scan
```
22 OpenSSH 7.2p2 ubuntu
80 Apache 2.4.18
1720 h323q931? 
5038 Asterisk call manager 5.0.2
```
Starting a gobuster scan
```
/images
/assets
```
On  the website there is a pyc script we can download and I decoded the script with `uncomple6` and now got the real script had to convert the script from python2 to python3 got the script to run and outputs Hello in ascii but this does not output everything from the script so I had ChatGPT help me modify it and now it has a text output that says
`Good job user admin the open source framework for builing communictions, installed on the server`
Doing a google search of the service we found on port `5038` there is a metasploit module to help us crack the login for the service and I got it to crack since we knew the username `admin:ab...` 
How to login into the service we need to telnet to the port 5038 and then enter this and hit enter afterword's
```
Action:Login
USERNAME:admin
SECRET:ab.....
```
and now we are logged into the service now we can try and enumerate the users with these commands
```
Action: Command
Command: sip show users 
``` 
and it has 3 users one has a username of `harry` and  shows his password in plain text `p4ss#.....` and this can be used to log into his SSH account and now we got access to the server there is a Example_Root.jar file I transferred the file to my machine and run `jd-gui` and looked into the jar file it looks like something is written to `/home/harry/root.txt` when the file `/tmp/flag.dat` exists and there is also a cron job running for a `run.sh` script in the `/root/java` folder so we can write something to the tmp file and wait about a minute and we should have the root flag 