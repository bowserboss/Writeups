Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.2p2
80 Apache 2.4.18
```
Going to the FTP it has anonymous login but there is nothing on the server and we cant put files on the server also so going to the webhost now taking a guess and there is a `robots.txt` file and it disallow a `/wordpress/` folder running a wpscan `version 3.9.14` plugins `mail-masta` users `btrisk,admin` googling the plugin we found a LFI and we can read files on the system `/wp-content/plugins/mail=masta/inc/campaign/count_of_sand.php?pl=/etc/passswd` the only user that is on the host is the `btrisk` user while messing with the LFI I was bruteforceing the logins for the website and got the admin login `amdin:admin` we can get a shell on the host this way after editing the `404` page and getting a shell as `www-data` we can find the `config.php` file and got the creds to the DB `root:rootpassword!` and we found creds for another user `btrisk:roottoor` and we can access that user now running `sudo -l` and we can just `sudo su` and now we are root 