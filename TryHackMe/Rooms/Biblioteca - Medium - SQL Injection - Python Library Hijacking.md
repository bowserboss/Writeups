Started with a nmap scan
```
22 OpenSSH 8.2p1
8000 Wrkzeug Python 3.8.10
```
Going to the webhost we get a login page when testing the login page when I do `amdin'` we get a 500 internal error sqlmap did not find anything tho starting a gobuster scan
```
/login
/register
/logout
```
We can make a account but for right now it does not get us any were so going back to this sql injection `ghauri` did find a blind time based SQL injection and we got one non normal database called `website` dumping the users table we got a login `smokey:My_P@.............` this login worked for the site and SSH looking around on the host I found the webapp folder in `/var/opt/app` and we have a `app.py` has a secret key `$upe..............` and a db login `smokey:$..........` doing some searching on the host agin and could not find anything to get to the user `hazel` but the username is the same for the password so now were hazel we can run hasher.py as root we can also set are own environment variables as root see looking back at the script we can hijack the python `hashlib.py` library and then point the path were we want to and we can exacute code as root so going to `/dev/shm` we make a file called hashlib.py and then write some python code to read the `/root/root.txt` flag and then set the path 
`sudo PYTHONPATH=/dev/shm /usr/bin/python3 /home/hazel/hasher.py` 
and now we have the root flag 