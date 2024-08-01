Started with a nmap scan
```
22 OpenSSH 7.6p1
1883 mqtt
8161 Jetty 7.6.9v20130131 ActiveMQ
```

Running gobuster on the webserver 
```
/admin/
/images/
/api/
/styles/
```
Nikto found default admin creds or the admin panel `admin:ad..n`
To read the messages we have to use a `mqtt` client with version `3.1 protocol`  if you can get this to work once you have shell can just read `chat.py` file for flag 3
The ActiveMQ that's running on the webhost `CVE-2016-3088` we can get a shell with metasploit and are user `activemq` but we can read the `shadow` so we can try cracking the ssh passwords for both users I left then running for a while and they never cracked
Running `sudo -l` we can run a python script as root user and we can edit the file so we can put a python reverse shell into the file and now we are `root` user
`sudo -u root /usr/bin/python3.7 /opt/apache-activemq-5.9.0/subscribe.py`
