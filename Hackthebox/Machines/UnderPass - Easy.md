Started with a nmap scan
```
22 OpenSSH 8.9p1
80 Apache 2.4.52
```
Going to the website we get a default apache page so going to start a gobuster scan 
```
/index.html
```
Did not find anything after a lot of searching so I went back to nmap and did a udp scan and found smtp port open and when I ran `snmp-walk` and in the hostname it says `daloradius` is the only server in the basin and that is a folder on the webhost but when we go to it we get 403 error so running a gobuster scan on this folder now shows we got free radius on the host and found a docker file `docker-compose.yml` and we also found a `/app` folder that gives us a 403 error going to run another scan on this as well and we find a couple more folders and then we get a login page when we go to `/app/operators` for `dalo redius` version `2.2 beta` did a google search for default login and found `adminstrator:radius` worked and now we are logged in when going to the user page we find a account with a md5 hashed password that cracked to `underwaterfriends` with a user `svcMosh` and now we can SSH into the host machine and when we run `sudo -l` we can run mosh server as root so looking into the help section of the program we can ssh into the localhost with the `--server` setting `mosh --server="sudo /usr/bin/mosh-server" localhost` and now we are root 