Started with a nmap scan
```
PORT     STATE SERVICE     REASON         VERSION
22/tcp   open  ssh         syn-ack ttl 63 OpenSSH 7.4p1 Debian 10+deb9u1 (protocol 2.0)
| ssh-hostkey: 
|   2048 77:00:84:f5:78:b9:c7:d3:54:cf:71:2e:0d:52:6d:8b (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCp5WdwlckuF4slNUO29xOk/Yl/cnXT/p6qwezI0ye+4iRSyor8lhyAEku/yz8KJXtA+ALhL7HwYbD3hDUxDkFw90V1Omdedbk7SxUVBPK2CiDpvXq1+r5fVw26WpTCdawGKkaOMYoSWvliBsbwMLJEUwVbZ/GZ1SUEswpYkyZeiSC1qk72L6CiZ9/5za4MTZw8Cq0akT7G+mX7Qgc+5eOEGcqZt3cBtWzKjHyOZJAEUtwXAHly29KtrPUddXEIF0qJUxKXArEDvsp7OkuQ0fktXXkZuyN/GRFeu3im7uQVuDgiXFKbEfmoQAsvLrR8YiKFUG6QBdI9awwmTkLFbS1Z
|   256 78:b8:3a:f6:60:19:06:91:f5:53:92:1d:3f:48:ed:53 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBISyhm1hXZNQl3cslogs5LKqgWEozfjs3S3aPy4k3riFb6UYu6Q1QsxIEOGBSPAWEkevVz1msTrRRyvHPiUQ+eE=
|   256 e4:45:e9:ed:07:4d:73:69:43:5a:12:70:9d:c4:af:76 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMKbFbK3MJqjMh9oEw/2OVe0isA7e3ruHz5fhUP4cVgY
25/tcp   open  smtp        syn-ack ttl 63 JAMES smtpd 2.3.2
|_smtp-commands: solidstate Hello nmap.scanme.org (10.10.16.4 [10.10.16.4])
80/tcp   open  http        syn-ack ttl 63 Apache httpd 2.4.25 ((Debian))
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Home - Solid State Security
| http-methods: 
|_  Supported Methods: HEAD GET POST OPTIONS
110/tcp  open  pop3        syn-ack ttl 63 JAMES pop3d 2.3.2
119/tcp  open  nntp        syn-ack ttl 63 JAMES nntpd (posting ok)
4555/tcp open  james-admin syn-ack ttl 63 JAMES Remote Admin 2.3.2
```
This machine is running `Apache JAMES` and its running version `2.3.2` and its vulnerable to `cve-2015-7611` with default creds `root:root` we can now auth to this `telnet 10.10.10.51 4555` and we can list users and change users passwords and then we can login as that user and read there emails the user who has the goods is `mindy` and then we connect to the email server `telnet 10.10.10.51 110` we can read her emails and get a login  
```
telnet 10.10.10.51 110 
Trying 10.10.10.51...
Connected to 10.10.10.51.
Escape character is '^]'.
mindy
root
+OK solidstate POP3 server (JAMES POP3 Server 2.3.2) ready 
-ERR
-ERR
USER mindy
+OK
PASS root
+OK Welcome mindy
HELP    
-ERR
list
+OK 2 1945
1 1109
2 836
.
RETR 2
+OK Message follows
Return-Path: <mailadmin@localhost>
Message-ID: <16744123.2.1503422270399.JavaMail.root@solidstate>
MIME-Version: 1.0
Content-Type: text/plain; charset=us-ascii
Content-Transfer-Encoding: 7bit
Delivered-To: mindy@localhost
Received: from 192.168.11.142 ([192.168.11.142])
          by solidstate (JAMES SMTP Server 2.3.2) with SMTP ID 581
          for <mindy@localhost>;
          Tue, 22 Aug 2017 13:17:28 -0400 (EDT)
Date: Tue, 22 Aug 2017 13:17:28 -0400 (EDT)
From: mailadmin@localhost
Subject: Your Access

Dear Mindy,


Here are your ssh credentials to access the system. Remember to reset your password after your first login. 
Your access is restricted at the moment, feel free to ask your supervisor to add any commands you need to your path. 

username: mindy
pass: P@55W0rd1!2@

Respectfully,
James
```
And we can use this login to SSH into the host machine but when we do it drops us in a rbash shell and we cant run a lot of commands but there is a exploit on `exploit-DB` that we can use and injections in the login banner of the ssh server a reverse shell we run the exploit then relog in and we got `mindy` but in a normal shell now we can get linpeas on the machine but the output did not have anything that stood out so going to switch to `pspy` but we need the 32 bit version and we have a python script being ran `/opt/tmp.py` that removes all the file in the `/tmp` but we can edit this script and add a reverse shell and then wait for about 3 minutes and we get a root shell back 