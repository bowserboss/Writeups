Started with a nmap scan
```
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 c2:5f:fb:de:32:ff:44:bf:08:f5:ca:49:d4:42:1a:06 (RSA)
|   256 bc:cd:e8:ee:0a:a9:15:76:52:bc:19:a4:a3:b2:ba:ff (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCenH4vaESizD5ZgkV+1Yo3MJH9MfmUdKhvU+2Z2ShSSWjp1AfRmK/U/rYaFOoeKFIjo1P4s8fz3eXr3Pzk/X80=
|   256 62:ef:72:52:4f:19:53:8b:f2:9b:be:46:88:4b:c3:d0 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKTxLGFW04ssWG0kheQptJmR5sHKtPI2G+zh4FVF0pBm
80/tcp   open  http    syn-ack ttl 63 Apache httpd 2.4.41
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://devzat.htb/
8000/tcp open  ssh     syn-ack ttl 63 Golang x/crypto/ssh server (protocol 2.0)
| ssh-hostkey: 
|   3072 6a:ee:db:90:a6:10:30:9f:94:ff:bf:61:95:2a:20:63 (RSA)
Service Info: Host: devzat.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
Checking out the website its a static website but there is some instructions to try out a chat we can try it out `sssh -l (username) devzat.htb -p 8000` anyone can connect to it i need to add this `-oHostKeyAlgrithms=+ssh-rsa` so I could connect but we cant do much with this atm doing a subdomain scan and we found one `pets.devzat.htb` and there is a `/.git/` folder on the webhost so we can use gitdumper to dump it all then we can review the source code and there is a command injection in the `pets` subdomain if we intercept the request and change the animal to `cat;id` and we get `patrick` at the user back so we can use this to get a reverse shell on the machine we can use a bash with the `bash -c` and then base64 encode it and now we got access as `patrick` this user also has a `id_rsa` key with no password we can use this to get a better shell we did not find much in the linpeas scan so we can try and connect to the chat as the user Patrick and we get a error `username is reserved for local use` so using the good shell we can do `ssh -l patrick devzat.htb -p 8000` and now we are on the chat app as the user I could not find anything but we know there is `InfluxDB` running on this host and its vulnerable to `cve-2019-20933` and we can use this to forge a jwt token to auth to the database and then we can dump the users and we got 3 users and one of them is a user on the machine `catherine` and now we can SSH into the host and we can see there is a port `8443` that the dev chat thing is running and now we can go to `/var/backups` and there is a backup to the dev bot we can copy it to `/tmp` and review the commands and find a password to use one command `/file` and we can use this for LFI and its running as the root user so we can read root `id_rsa` key and then ssh into the root account 