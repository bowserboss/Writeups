Started with a nmap scan
```
PORT    STATE  SERVICE REASON         VERSION
22/tcp  open   ssh     syn-ack ttl 63 OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 22:75:d7:a7:4f:81:a7:af:52:66:e5:27:44:b1:01:5b (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDFgr+LYQ5zL9JWnZmjxP7FT1134sJla89HBT+qnqNvJQRHwO7IqPSa5tEWGZYtzQ2BehsEqb/PisrRHlTeatK0X8qrS3tuz+l1nOj3X/wdcgnFXBrhwpRB2spULt2YqRM49aEbm7bRf2pctxuvgeym/pwCghb6nSbdsaCIsoE+X7QwbG0j6ZfoNIJzQkTQY7O+n1tPP8mlwPOShZJP7+NWVf/kiHsgZqVx6xroCp/NYbQTvLWt6VF/V+iZ3tiT7E1JJxJqQ05wiqsnjnFaZPYP+ptTqorUKP4AenZnf9Wan7VrrzVNZGnFlczj/BsxXOYaRe4Q8VK4PwiDbcwliOBd
|   256 2d:63:28:fc:a2:99:c7:d4:35:b9:45:9a:4b:38:f9:c8 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBAsf1XXvL55L6U7NrCo3XSBTr+zCnnQ+GorAMgUugr3ihPkA+4Tw2LmpBr1syz7Z6PkNyQw6NzC3KwSUy1BOGw8=
|   256 73:cd:a0:5b:84:10:7d:a7:1c:7c:61:1d:f5:54:cf:c4 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILMrhnJBfdb0fWQsWVfynAxcQ8+SNlL38vl8VJaaqPTL
80/tcp  open   http    syn-ack ttl 63 Apache httpd 2.4.6 ((CentOS) PHP/5.4.16)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.4.16
443/tcp closed https   reset ttl 63
```
Going to the site its just a blank page with some text on it and if you look at the source it says that `upload` and `gallery` is not yet linked so going to run a gobuster scan 
```
/uploads              (Status: 301) [Size: 236] [--> http://10.10.10.146/uploads/]
/photos.php           (Status: 200) [Size: 1302]
/upload.php           (Status: 200) [Size: 169]
/lib.php              (Status: 200) [Size: 0]
/index.php            (Status: 200) [Size: 229]
/backup               (Status: 301) [Size: 235] [--> http://10.10.10.146/backup/]
```
and going to `/backup` give us a file called `backup.tar` which is a backup of the site and looking at the source there is some file extensions PNG is one of them so we can try and file upload bypass and we can see what we upload in the `/photos.php` page so I uploaded a png file and add some PHP code in it and then intercepted the request and add `.php.png` to the file name and then tested it `?e=id` and got `apache` user back we can use a base64 encoded payload and now we are on the machine we found a interesting file in the `guly` home folder `crontab.guly` shows this php script is running on a cron every 3 minutes looking at the php code we can control the input into a message field first we need to put are reverse shell in base64 with the `-w0` option and then `touch '/var/www/html/uploads/a; echo base64 | base64 -d | sh; b'` and then we have to wait 3 minutes for the script to run  if we run `sudo -l` there is a script we can run a root user reading the script if we type `a` and then something after that it takes both commands so if we do `a id,a ls /root/,a pwd,a whoami` and then run it agin `bowser,a /bin/bash,b,c` and now we are root user 