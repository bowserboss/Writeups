Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
```
Going to the website its a basic apache default page so going to do a gobuster scan
```
/music
/artwork
/ona
```
I found a music folder when going to the music site its `solmusic` when we try to login in its logs us in as guest but we can login in `admin:admin` and now we are admin and we find a domain linked to the site `openadmin.htb` and its also running `OpenNetAdmin` version `18.1.1` there is a RCE exploit for this version now that we exploit this we got to the `www-data` user looking for the database config found the login for the db `ona_sys:n1nj4W4rri0R!` and this password is also the password for `jimmy` running linpeas as this user shows there is a internal app running on port `52846` we can port forward this to are host `ssh jimmy@openadmin.htb -L 5000:localhost:52846` and when we go to port `5000` on are host we get a login page since we have access to the file share looking at the code for the site in the `main.php` there is a user jimmy and a sha512 password that we can try and crack if we use jumbo john we can crack it `jimmy:Revealed` and when we login there is a `id_rsa` key now we have the password for the user `joanna:bloodninjas` and now we can ssh as this user when we do `sudo -l` we can run nano as root and if we go to gtfobins it gives us how to get root with nano 