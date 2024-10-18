Started with a nmap scan
```
22 OpenSSH 8.2p1
3000 ppp?
10259
10255 Goland http server 
16443 web server? 
25000 gunicorn 19.7.1
31337 nginx 1.21.3
32000 Docker Registry
```
Starting with a gobuster on the site with port `31337`
```
/index.html
/assets
/css
/vendor
/.git-credentials
```
in the `.git-credentials` file there is some creds and if we url decode it we get some creds we can ssh into the host machine I put linpeas on the host and did not find anything but when we ran kube-hunter we found  a privliged pod that we can access and do this exploit on it `CVE-2019-15789`running the commands the host can be super slow when we run this command it will show us the pods
`microk8s kubectl get pod`
and then run this command 
`microk8s kubectl exec -it (podname) /bin/bash` 
and then we can go to `/opt/root/root` and get the final flag 