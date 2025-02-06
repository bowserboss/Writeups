Started with a nmap scan
```
21 vsftpd 3.0.3
80 Apache 2.4.29
```
The FTP does not have anonymous login and going to the webserver its running wordpress and looking at the source code of the site it has a domain called `http://adana.thm/` when we run wpscan we have to add `/index.php/` as the content directory and then wpscan will run  
```
Theme 
twentynineteen

Plugins
non that are vulnerable 
```
Gobuster scan
```
/announcements/
/phpmyadmin/
```
And in the announcements folder there is a image and a wordlist running binwalk showed nothing but when I ran stegseek is blocked by a password `123adanaantinwar` and has a txt file called `user-pass-ftp.txt` and it has a base64 encoded login `hakanftp:123adanacrack` when seeing what files are on the ftp its the website files so we can just upload a reverse shell and get a shell but does not seem to work so downloading some files in the `wp-config.php` there is `phpmyadmin` creds `phpmyadmin:12345` looking around the pma databases there is two of them and a user with a hashed password but you cant crack it then looking at the `wp-options` and then `site-config` there is a subdomain to are site `subdomain.adana.thm` and its the site from the ftp server and now we can get are reverse shell as `www-data` the web flag is in the `/var/www/html` folder ran linpeas after that did not find much but we now we need to get to the text `hakanadey` so we need to get `sucrack` onto the host we can download it then transfer it with http server and then run `dpkg -x sucrack sucrack` and now going back to that wordlist we can take the `123adana` to the beginning of every word in that list cracked the password after a little bit `123adabasybaru` running linpeas there is 2 unknown binary's and one called binary if we run it and enter the prefix of the password it try's to kill 2 processes so then I ran a ltrace and got the password so I entered that and then makes a jpg file in are home folder called `root.jpg` then we can get this onto are system looking at the image if we run xxd at the top there is something off there is a string and if we do `from hex to base85 ` we get the root login 