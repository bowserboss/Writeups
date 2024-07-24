Started with a nmap scan
```
21 vsftpd 3.03
22 OpenSSH 7.6p1
80 Apache 2.4.29
```
Running a gobuster scan on the website
```
/images
/css
/robots.txt
```

Test to see if the FTP has anon login enabled it does not so going to the website now just clicking on links to see if they go any were any of the links you click goes to a `post.php` file with a parameter of `post=` we can use this to read any files on the host machine 
`http://10.10.195.250/post.php?post=/etc/passwd` 
There is a file in the robots.txt so using the LFI we can read the file and its creds for the ftp server 
`ftpuser:givemef.....` path it saves the files to `/home/ftpuser/ftp/files` from this we can upload files to the ftp and then use the LFI to run a php reverse shell and got a call back now to get access as toby we can run `sudo -u toby bash` and now we are toby and his flag is in his home folder to get access as mat we can edit the `cow.sh` file in the jobs folder under toby and get a call back as mat when the cron jobs exacute the script now to get to the will user we will run another reverse shell we can run the `will_scripy.py` and we can edit `cmd.py` and out a python reverse shell and then use this command to exacute the script with sudo
`sudo -u will python3 /home/mat/scripts/will_scripts.py cmd.py`
and now we are will and time to get root searching the file system for a bit I found a `backups` folder in the `/opt` and has a file called `key.b64` and its encoded in b64 used cyberchef to decode it and its a ssh key
`sudo root@10.10.195.250 -i id_rsa`
and now we are root


Flags: 
1. on main site robots.txt
2. flag_2 ftp user
3. flag_3 /var/www/html/more_secrets_a9f10a/
4. /home/toby/flag_4.txt
5. flag 5 is in mat home folder
6. flag 6 is in will home folder
7. flag 7 is in the root folder