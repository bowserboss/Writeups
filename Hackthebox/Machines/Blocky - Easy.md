Started with a nmap scan
```
21/tcp    open   ftp       syn-ack ttl 63 ProFTPD 1.3.5a
22/tcp    open   ssh       syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu 
80/tcp    open   http      syn-ack ttl 63 Apache httpd 2.4.18
8192/tcp  closed sophos    reset ttl 63
25565/tcp open   minecraft syn-ack ttl 63 Minecraft 1.11.2 (Protocol: 127, Message: A Minecraft Server, Users: 0/20)
```
The nmap scan showed the webserver redirects to `http://blocky.htb` going to the FTP it does not have anonymous login going to the webserver now its running wordpress its running version `4.8` when running wpscan it found a username called `notch` going to start a gobuster scan of the webhost 
```
/wiki                 (Status: 301) [Size: 307] [--> http://blocky.htb/wiki/]
/wp-content           (Status: 301) [Size: 313] [--> http://blocky.htb/wp-content/]
/wp-content/uploads/2017/
/plugins              (Status: 301) [Size: 310] [--> http://blocky.htb/plugins/]
/wp-includes          (Status: 301) [Size: 314] [--> http://blocky.htb/wp-includes/]
/javascript           (Status: 301) [Size: 313] [--> http://blocky.htb/javascript/]
/wp-admin             (Status: 301) [Size: 311] [--> http://blocky.htb/wp-admin/]
/phpmyadmin           (Status: 301) [Size: 313] [--> http://blocky.htb/phpmyadmin/]
```
While enumerating the webhost in the `/plugins` folder there are two jar files one called `BlockyCore.jar` and `griefprevention` going to start reversing the `Blockycore` file since when I ran strings is a  lot smaller file with less stuff then the other one when opening it in `jd-gui` there is a sql password of `...............` and that is also for the ssh access to the user `notch` running `sudo -l` we can just become root with no password `sudo -u root su` 