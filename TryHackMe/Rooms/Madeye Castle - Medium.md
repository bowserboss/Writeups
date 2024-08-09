Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
139 Samba 3-4
445 Samba 4.7.6- Ubuntu
```
Looking at the smb first there is a null share we can use to enumerate signing False SMBv1:True
```
netexec smb 10.10.233.119 -u '' -p '' --shares
SHARES
print$
sambashare
IPC$
```
Ran enum4linux it got 2 users
```
harry
hermonine
hogwartz
administrator
```
Tried connecting to `sambashare` using this command
`smbclient -N \\10.10.233.119\sambashare`
It has 2 files on the share `spellnames.txt` and `.notes.txt` 
the notes file says  Hagrid says spells names are not good since they will not `rock you` and Hermonine loves historical text editors and books

Starting a gobuster on the webhost 
```
/backup
/backup/email
---- Hogwartz------
/login
/static
/logout
```
If you go to `backup/email` its a email chain that says there is a domain of `hogwarts-castle.thm` and they think that virtual hosting is cool also in the email chain they changed the username of `hogwarts` but changed the s to a z so its `hogwartz` and on the default apache page has a TODO list says virtual hosting is good and has a different domain of `hogwartz-castle.thm` 
Now were going to see if there is any subdomains on the webhost did not find any 
The new domain we found has a login page and when i put a ' in the username field and gives me a internal server error so were going to take the request and run it in sqlmap It did find it vulnerable but did not pull anything so I tried doing manually and finally got to the users columns and get something out of it
`' union slect group_concat(password),2,3,4 FROM users -- -` 
And got a message saying password for and gives a hash we can use this and send the request to burp and use a list to enumerate the database we found some we can look at now 
```
password
notes
name
```
On notes one it has a hint for us it says the username is the first name and password users `best64` and now we have a list of usernames and now we can use that `spellnames` list and hashcat has a rule called best64 so we can make a list of possible passwords to try and crack the hashes we cracked one hash password is `wingard.......3` and when we take a username from the list we got we got a different error but same one we say in the notes we take the first username we found `harry` and try it on the ssh with the password we cracked and now we have SSH access doing `sudo -l` hermonine can run pico and we can use this to read files and read the user2 flag and we can run 
`sudo -u hermonine /usr/bin/pico -s /bin/sh` and then ctrl T and now we got shell as hermonine we have a unknown SUID binary called `/srv/time-tunrner/swagger` and we open this in a debugger or you can just run strings and see its making a random number and compares it to ares and its calling a command `uname -a` but does not use a full path so we can make a file in the tmp folder to read the flag name it uname and then set the file to the path of the tmp folder and then we can run the time-tunrner and it will out put the flag