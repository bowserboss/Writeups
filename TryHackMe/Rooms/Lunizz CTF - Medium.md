Started with a nmap scan
```
80 Apache 2.4.29
3306 MySQL 5.7.33 ubuntu
```
Started a gobuster scan
```
/hidden
/hidden/index.php
/hidden/uploads/
/whatever
/instructions.txt
```
On the hidden folder on the webhost we can upload pictures that are small in size and says its uploaded to `uploads/` I found a txt file with what looks like default creds for the mysql user
`runcheck:CTF_script_........` so can now login into the mysql server but I had to set SSL to false `--ssl=FLASE` and there is one database called `runornot` and there is a section set to `0` and from the `/whatever` folder on the webhost is a command executer that is also set to `0` to do so we can run these 2 commands `update runcheck` and then `set run = 1;` and now its set to `1` and now we go back to the `/whatever` and we can exacute commands on the host system and from this we can get a reverse shell I found a file in the `/proct/pass/bcrypt_encryption.py` that has a hash in the file and we can try to crack it there is also a webserver running on port `8080` on the local host we went the route after looking at the webserver I gonna try cracking the hash its in bcrypt and will take a while made a python script or can use hashcat to crack it and its for the user `adam` and then we can get root with PwnKit 