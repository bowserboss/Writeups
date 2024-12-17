Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
111 RPC
2049 nfs
```
Looking at the NFS service first there are two paths we can mount `/home/ross` and `/var/www/html` when we mount the home folder we have access to ross home folder now looking in the Document folder there is a keepass file but `keepass2john` does not support the version it is so now going to mount the web folder we can mount the `/var/www/html` folder but we need to make a test user on are system and then change the uid with usermod to `2017` and once we do that we can now read the mount and now we can add file to the webhost and get a reverse shell and we get a callback as the user `alex` now we need to remount the ross folder and change are uid to `1001` and we can read the files and there is a `.Xauthority` file and ross is using display `:0` and we can use a screen shot with xwd and then download it to localy look at it and we get the password for the root user 