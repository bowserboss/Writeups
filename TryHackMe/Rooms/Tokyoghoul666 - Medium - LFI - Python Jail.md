Started a nmap scan
```
21 vsftpd 3.0.3 anon login
22 OpenSSH 7.2p2 Ubuntu
80 Apache 2.4.18
```

Going to the web site and looking at the source there is a hint that says anon login on the ftp which we know from the ftp scan so I went to the FTP and found 1 file and another folder with a 2 files in it one if a ELF file and we can open it in `ghidra`  and found the password for the file `kam...` running the program and giving it the password it gave me what it says im looking for `You_....t` turns out its the password for the jpg file that was also on the ftp it was hiding a note in the pic the note has morse code that decodes to hex then to base64 and the end of the note said it will be a secret directory `d1r3c70....` 
Doing a gobuster scan in the hidden folder
```
/claim
```
I did not find anything in  the claim folder going to the webpage if we click on NO or YES will will take us to a link with `view=` this we can use for LFI you can use hydra to brute the pram
`http://10.10.130.71/d1r3c70ry_center/claim/index.php?view=/../../../../../../../../../etc/passwd`
and in the passwd file there is a user with there hash which is not normal username is `kami....` and we can try cracking the hash and the password is `passw....` now we can SSH into the host and get the user flag now time to go for root there is a script in are folder `jail.py` and we can run this file with sudo and doing a google search there is a article about escaping these jails to read files and we can read the root flag with this 
`__builtins__.__dict__['__IMPORT__'.lower()]('OS'.lower()).__dict__['SYSTEM'.lower()]('cat /root/root.txt')`
