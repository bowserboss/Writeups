Started with a nmap scan
```
22 OpenSSH 7.2p2
25 Postfix
80 Apache 2.4.18
339 OpenLDAP 2.2.x-2.3.x
443 Apache 2.4.18
5667
```
Looking at the webhost port `443` and `80` are the same site `Nagios XI` looking online we found a default username but it says the password is set at install so we tried guess the password a couple times and got it `nagiosadmin:admin` and we can log in and we get a message saying `dave we cant do that` and also looking online now for a exploit and I found `CVE-2020-33578` works and there is a PoC on exploit-DB `49422` and using this we we can get a shell as `www-data` but this exploit only gets us to a user we can not use I found another exploit `CVE-2019-15949` and this is also a auth RCE but this one will get us to the `root` user and there is only one flag for this machine 