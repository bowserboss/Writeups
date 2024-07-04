Started with a nmap scan 
```
22 OpenSSH 8.2p1 Ubuntu
80 Apache 2.4.41
```

Starting a gobuster scan with list 2.3-medium
```
/images
/spip
```
Starting a nikto scan
```
/images
```

Clicking around the site there is not much its just one page all the links go back to the main page 
Going to the `/spip/` folder we find the real site and the SPIP version is `4.2.0` you can get the version for SPIP by looking at the source code from the webpage there is a unauthenticated RCE for this version `CVE-2023-27372` you can also use metasploit to get the shell

Doing some basic looking around we can access the user `think` and he has a `.ssh` folder that has a `id_rsa` and get better shell access running linpeas on the server had to go to `/dev/shm` to run it
we found a unknown binary under the SUID/SGUID `/usr/sbin/run_container` it looks like it manages all the docker images on the host we can run strings on the program and find it exacute a script in the `/opt` folder looks like we can modify this script `/opt/run_container.sh`  but we cant read anything in the /opt folder we can see apparmor is enabled and are shell is not bash its ash and then using this command we can spawn a unconfined bash shell `/lib/x86_64-linux/ld-2.31.so /bin/bash` and now we can get into the `/opt` folder and we can inject `bash -p` into the `run_container.sh` script now we can run the script and start the container and we got root