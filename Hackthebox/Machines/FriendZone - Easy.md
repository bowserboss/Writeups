Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.6p1
53 DNS 
80 Apache 2.4.29
139 SMB
443 Apache 2.4.29
445 SMB
```
Domain `friendzone.red` Hostname `FRIENDZONE` 
The FTP has no anonymous access but we can use SMB guest account and we have READ on `general` share and `READ,WRITE` on `Development` share in the general share we have a `creds.txt` file for admin creds on something `admin:WORKWORKHhallelujah@#` now checking out the dns if we do `dig axfr friendzone.red @10.10.10.123` we get some subdomains 
```
administrator1
hr
uploads
```
We can only access them over https these creds we found work for the admin page and there is a picture upload on the uploads subdomain and then under the admin page we can access this picture messed around with the parameters it gave us cant seem to find anything but two pictures going to run a gobuster scan on the admin domain 
```
/images
timestamp.php
```
in this timestamp parameter we can get LFI and if we run the nmap script agin we can see the full path of the dev share and can access files `/etc/Development` and we can put a reverse shell on the share since we have write access `/dashboard.php?image_id=a.jpg&pagename=../../../etc/Development/rev` we can see from the source code we could read the files and they strip the `.php` of the files so now we have access as `www-data` looking in the `/var/www` folder there is a `mysql_data.conf` file which has creds for the mysql service they also work for the user account `friend:Agpyu12!0.213$` running `pspy64` root is running a script we saw in the linpeas output `/opt/server_admin/reporter.py` we cant edit the script though and it tries to send a email to the `amdin1@friendzone.red` and this script is using python2 and importing `os` and we can edit `os` we can add a shell to this and get root if we edit the end of the `os.py` file and add the python reverse shell from revshells and the import and everything and wait for the script to be ran and now we are root 