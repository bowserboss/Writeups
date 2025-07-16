Started with a nmap scan
```
22 OpenSSH 8.2p1
5000  Gunicorn 20.0.4
```
Going to the website its running a python code testing using ace editor and has some blacklist its using for certain python syntax 
Known blacklist
```
import os
import base64
import subprocesses
Turns out import is blocked
exec 
eval
```
After lots of testing I got some statement to work to fetched stuff from the db
```
To get usernames: print([u.username for u in db.session.query(User).all()])  
To get password hashes: print([u.password for u in db.session.query(User).all()])
```
and now we have the usernames and hashes from the database time to try and crack them 
```
devolopment:devolopment
martin:nafeelswordsmaster
```
and testing these logins on SSH martin was able to login when running `sudo -l` we have perms on `/usr/bin/backy.sh` with no password we can edit the `task.json` file and edit the backup folder that it wants to backup `/home/../../root` and since we have everything in the root we can take the `id_rsa` key and ssh into root to get the user flag 