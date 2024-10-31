Started with a nmap scan
```
22 OpenSSH 7.6p1
111 RPC
3000 ppp? Grafana web server 
5000 Werkzeug 2.0.2 Python 3.8.12
6443 Kubernets 
```
Did a gobuster scan on port `3000` 
```
/login
/public
/signup
/api
/robots.txt
```
We do have default creds on this webhost for the Grafana instance `admin:admin` 
Doing a gobuster on the web host on port `5000` there is only one page and its to exploit this were going to need a LFI
```
/console
```
So going back to the other webhost its running Grafana version `8.3.0` which is vulnerable to `CVE-2021-43798` which is a LFI and we can read files like `/etc/passwd` so we can read files for the console bypass 
On the webhost on port `5000` in the `main.css` file there is a pastebin link which has a base64 encoded message that has the username `vargrant` and we got the password from the LFI we found in the `/etc/passwd` file `her......` now we can ssh into the host running `sudo -l` we can just go to root user and now we can start enumeration on the pods there is a flag in the `k8s.authentication` doing some searching around the filesystem I found a `.git` folder hidden deep in the filesystem which had one of are flags we need in it to get the final flag there is a pod named internship and if we get the job and output in json format there is the final flag in the output its a SHA1 hash cracked to `.......` 