Started a nmap scan 
```
22 OpenSSH 8.9p1 Ubunutu
80 nginx 1.18.0
5000 Internal Http server 
```
On the webserver there is a redirect to the domain `http://editorial.htb` 
Starting a gobuster scan with list `2.3-medmium` 
```
/about
/upload
/upload-cover
```
Fuzzing for subdomains did not find anything
After looking for a while and testing I found SSRF in the upload page in the book url when you hit preview we can test this with this payload `http://127.0.0.1/` and when get a static image back but all we can do is download it so now ima try and port scan with burp and see if there are any internal ports we found a hidden internal port we did not know about `5000` it gives a different response now if we hit the api endpoints using the ssrf on the localhost we can read them and when we hit api end point `/api/lastest/metadata/messages/authors` and it gives as a login to a dev site but it also works for the ssh user `dev:dev0802.....` when looking in the `.git` folder and looking back at old comments to the code there is one with a description to changing from prod to dev and there are some creds if you run a `git diff (hash)` `prod:080217_Pro......` and these creds work for the prod account for SSH and when we do sudo -l we can run a script called `clone_prod_change.py` and there is a library called gitpython that is vulnerable to `CVE-2022-24439` and we can read the root flag this is the command for root flag `sudo /usr/bin/python3 /opt/internal_apps/cloone_changes/clone_prod_change.py 'ext::sh -c cat% /root/root.txt% >% /tmp/root'` 


















