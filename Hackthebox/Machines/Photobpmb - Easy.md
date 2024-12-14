Started with a nmap scan
```
22 OpenSSH 8.2p1
80 nginx 1.18.0
```
There is a domain linked to the webhost `photobomb.htb` going to the website it says are creds are in a welcome pack but when looking at the code of the site there is a js file with are creds in it `pH0t0:b0Mb!` and now we are loged into the site we have blind command injection when we post to the printer page with the parameter filetype we can make a rev.sh file and download it to the host and run it we get a call back as `wizard` if we run `sudo -l` we can run a script as root `/opt/cleanup.sh` if we look in the bashrc file we can see how to exploit it for root and we can make `]` into bin bash and then set the path for the file when we run the script and now we are root 