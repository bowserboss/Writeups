Started with a nmap scan
```
PORT    STATE SERVICE REASON         VERSION
22/tcp  open  ssh     syn-ack ttl 63 OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 19:97:59:9a:15:fd:d2:ac:bd:84:73:c4:29:e9:2b:73 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDDf7K92wk79uG+iF3K0XaLWtJBrMKM/PfpE01tmpOSxwdhbiXQZ1ggfXFOfjSrkNqO/W3apn2SH1IO3jRCUGmEfXUzmTlX7FKDETKKFJuSZFwdphEqxoX/wCZ+NQhBX9bMT817GjQTNPEmkQsuWUD7PcVBYhRSKohP0jbAc464VKbSeiQt6q1I71CxzUtqMnL7pOREvF41+0K0BNtQUJVKxq5Aq0g67Ba8b0UEecOwgS8O4rZeKrfuYHMXnl6n32XrjqliowOSaZl/iYOu3dgkooIEDFDiaEapOW7J71/Ag/96NWzUf1U91QxCIA2GNtAhXT+Bn+ncbFtWxGdh6enL
|   256 88:58:a1:cf:38:cd:2e:15:1d:2c:7f:72:06:a3:57:67 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ2F5pumDKrMj6rP99uQehJ2kGbw7z54Ydq7uuZ8FgoTw7wJ44SSytCh1jkrQay1jRg0+4Izw0cqUeW93J5kDCc=
|   256 31:6c:c1:eb:3b:28:0f:ad:d5:79:72:8f:f5:b5:49:db (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGwL0BBmHKyKlj3sRMsytml1etD3lMtofGbdxD8aAh1T
80/tcp  open  http    syn-ack ttl 63 Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips mod_fcgid/2.3.9 PHP/5.4.16)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Lightweight slider evaluation page - slendr
389/tcp open  ldap    syn-ack ttl 63 OpenLDAP 2.2.X - 2.3.X
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=lightweight.htb
| Subject Alternative Name: DNS:lightweight.htb, DNS:localhost, DNS:localhost.localdomain
| Issuer: commonName=lightweight.htb
| Public Key type: rsa
| Public Key bits: 1024
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2018-06-09T13:32:51
| Not valid after:  2019-06-09T13:32:51
| MD5:   0e61:1374:e591:83bd:fd4a:ee1a:f448:547c
| SHA-1: 8e10:be17:d435:e99d:3f93:9f40:c5d9:433c:47dd:532f
```
Going to the website and looking around we find a `/users.php` page and it adds are ip and a username and password to the SSH and now we can SSH into the host machine we can run linpeas now and see whats going on on this machine we have cap ability's over `/usr/sbin/tcpdump` we have `cap_net_admin,cap_net-raw+ep` if we do `/usr/sbin/tcpdump -i lo` we can capture traffic and we can write it with `-w` to a pcap file and then get it onto are host machine and open it in wireshark and we can see the creds for a user called `ldapuser2` and we can su into this user now and they have a backup zip file in there home folder and we can get it back onto are host and then use `7z2john` to get it in a hash format so we can crack it and it cracked to `delete` and we can extract it now and we got some `php` files and in the `status.php` there are creds for the user `ldapuser1` now running linpeas as this user we have full cap ability's on `openssl` and we can abuse this and add are self to the `sudoers` file 
```
/home/ldapuser1/openssl enc -n /etc/sudoers > /tmp/sudoers
/home/ldapuser1/openssl enc -in /tmp/sudoers -out /etc/sudoers
```
now and we just do `sudo su` and we are root 