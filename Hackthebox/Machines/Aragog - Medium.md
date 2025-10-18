Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.2p2
80 Apache 2.4.18
```
There is a domain for the site `http://aragog.htb` 
Looking at the FTP first it does have anonymous login and one file on it `test.txt` inside the file there a subnet mask and we cant put files on to the ftp and the ftp is not linked to the website
Checking out the website its a default apache page cant seem to find anything looking back at the txt file its in xml format and this machine was made in 2018 so looking for exploits in 2017 there was a exploit for `apache struts xml` `CVE-2017-5638` but we need a target uri also so back to running gobuster scans and we find a php file `/hosts.php` all the poc we can find online wont work we have to exploit this manually and the format is the `test.txt` file so we need to take the request send it to burp and chnage it from GET to POST and then add this
```
<?xml version="1.0"?>
	<!DOCTYPE svg [<!ENTITY test SYSTEM 'file:///etc/passwd'>]>
	<details>
		<subnet_mask>
		&test;
		</subnet_mask>
		<test>
		</test>
		</details>
```
and now we can see the `/etc/passwd` file we have two users `cliff,florian` so testing the ssh after a while and its public key denied which normally means there is a `id_rsa` some were on the host and after checking the users we found one in `florian` and we can read it with the exploit so now we can ssh into the machine looking around the web folder has permissions of `777` which is not normal and there is a wordpress site also running on this machine in `dev_wiki` we can access the database from getting the creds in the `wp-config.php` file but we cant crack the hash for the user running `ps aux` there is a binary running on the host called `woopsie` and it is logging into the wordpress site so we can edit the `wp-login.php` file to capture the creds and these creds can be used to log in as the root user 