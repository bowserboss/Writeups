Started with a nmap scan
```
21 vsftpd 3.0.2
22 OpenSSH 7.4
80 Apache 2.4.6 CentOS
139 smb
443 Apache 2.4.6 CentOS
445 Samba 4.10.16
```
There is nothing on the FTP server looking at the smb shares there is one called `temporary share` and there is a file in there called `message-to-simeon` says
```
that he is moveing files insurcely and to put them into /opt and that his password is insurce as well
```
The webhost both go to a default apache page starting a gobuster scan of the host
```
/simeon/
/cgi-bin
```
I tried cracking the ssh and smb with rockyou but it was leading to nothing so I got the idea to try and make a wordlist from some of the chapters on the smb we can use this command to make a list `cat text1.txt | tr ' ' '\n' > output.txt` and we can use this list to try and crack the ssh agin and it cracked this time login for SSH account is `simeon:.........` found a username and password in a `.htaccess` file cracked the login to `theodore:........` but did not work for anything also found a RSA private key in the smb under the `/home/simeon/chapter7/paragraph7.1/text2.txt` I put linpeas on the host and we can run tcpdump and have special cap permissions so I put `pspy64` on the host to see if there is anything else going on seeing Theodore is running a python script called test-www-auth.py so lets run the tcpdump now on the lo interface and we can see a GET request coming in with a Authorization Basic which has a hash of base64 and when we decode it its creds for Theodore ssh account `theodore:Rijyaswa......` and when we run `sudo -l` we have sudo access to a script called `infra_as_code.sh` that runs ansible rules looking in the ansible folder and the script looking into the yaml files the httpd one had something interesting in it has a role set and we can edit this role looking in the roles folder there is a + sign when I run `ls -la` on the `configure-RedHat.yml` file so we can edit it and set a reverse shell in a bash script 
```
- name: root
  command: sudo bash /tmp/shell.sh
```
And now we are root


Possible usernames

```
simeon
theodore
automation
```

