Started with a nmap scan
```
22 Openssh 7.2p2 ubuntu
5581 vsFTP 3.0.3
5752
7331 Apache 2.4.18 
```
Started with the ftp service it does allow anonymous login with 2 files
```
.file
marked.txt
```
so the `.file` is a complied python script and the `marked.txt` is saying that he is trapped in the `/home/vekay/ftp` folder need to get the backdoor key to the map
Using the `uncompyle6` program to decompile the python script had to change the name to `file.pyc` and it decoded the script to readable output gave us a username and passowrd and its connecting to something on port 5752 so the login is encoded in something I had ChatGPT make a python script to decode the login and this is what I got
```
1337-h4x0r
n3v3r_g0nn4_
```
Connecting to the service on port `5752` using `nc`  it ask for a username and password and then gives this output
`t3mple_0f_y0ur_51n5` 

Doing a gobuster scan on the webserver now
```
/assets
/assets/style.css
/private.php
/t3mple_0f_y0ur_51n5.php
/t3mple_0f_y0ur_51n5.html
```
Tried fuzzing  for parameters on the `private.php` file but cant find nothing going to try and fuzz the `assets` folder in the assets folder there is a style.css file that has a base64 encoding string and the note says something about a key and making guards COLLIDING with each other using what we thought might be a key is really a php file on the host and there is a html file as well after a while of doing SHA-1 hash collision finally got it to output  a name of a txt file 
`m0td_f0r_j4a0n.txt` 
We got a note that he left his private key for us and 2 possible usernames `j4x0n` and `h4rdy` with the ssh key we could access the user h4rdy but we get put into a restricted shell to bypass this we need to add the `-t bash --noprofile` and then export are bin so we can run commands `export PATH=$PATH:/bin:/usr/bin` and now we can run commands like normal running sudo -l the user j4x0n can run `/bin/cat` as sudo so we can get the flag `sudo -u j4x0n /bin/cat user.txt` and then we know he has a .ssh folder and has a id_rsa key in there cat that out and now we can get to user j4x0n 
And we got root using PwnKit 