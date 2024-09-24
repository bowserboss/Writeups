Started with a nmap scan
```
22 OpenSSH 7.6p1
111 rpcbind
8080 Werkzeug httpd 0.14.1 python 3.6.9
58379 mountd
60431 mountd
```
So I'm going to start with the nfs shares we seen in the nmap scan doing `showmount -e 10.10.142.24` and we get one share called `/var/nfs/general *` and we can mount this to are tmp folder and when we go in it we see a `credentials.bak` file in the file it has some creds to something did not work for SSH so lets go to the http server now so we just get a static page when we go to the host so lets start a gobuster scan and see what we can find
```
/login
/cat
```
That login we found works for the website and says we can test are payloads so I tested with a ping and it called me back and then I put a reverse shell and got a shell now looking around a little bit we can go into the users `szymex` and there is a script called `SniffingCat.py` this script has the password in it but its rot13 encrypted when we decrypt it its `cher.......` now going to tux folder there is a 2 folders the first one we can compile the code or just look at it and get the first part of the hash and then go to folder 2 and there is a pgp key and file we need to get onto are machine and then run these commands to read the file `gpg --import private.key` and then run `gpg fragment.asc` and we get the 3rd part of they in the file it outputs and then we need to run the find command and the second part of the key is in the `/media` folder and then going back to folder 3 it just gives us the last part and now we need to decode it and we cracked it to `tuxykitty` and now we are the user `tux` so when we do `sudo -l` we can run a script as the user `varg` and when we run it it boots up a cooct os and makes a folder in the `/opt `folder and it has `.git` folder as well we can run `git diff hash` and we can see the login script that has a password of `slowr......` and that's the password for `varg` now time for root so we can run umount as root so lets check for mounted folders and we got some but the one we want is `/opt/CooctFS ` so we un mount it and then it shows the root folder and there is a .ssh folder with a `id_rsa` and we can use this to ssh into root it has no password 