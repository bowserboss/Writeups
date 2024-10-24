Started with a nmap scan
```
22 OpenSSH 8.4p1
80 nginx 1.18.0
```
The web server in the nmap scan redirected to a domain `http://app.blurry.htb` 
Doing a subdomain scan
```
app
files
chat
api
```
Looking at the site with app if we just give it a name and then it logs us in I searched on google and found a exploit on Hacking clearml and got a user as `jippity` and there is a `id_rsa` file so we can have a good stable shell access when we do `sudo -l` we can run `evalute_model` in the folder `/models/*.pth` so there is a model script you can modify to get a reverse shell and then we can compile it on the machine and then move it to the `/models` folder then run this command `sudo /usr/bin/evalute_model /models/demo.pth` and now we have a reverse shell as root 