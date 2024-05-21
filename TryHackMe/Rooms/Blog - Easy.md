Started with a nmap scan
```
22/tcp   open     ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 57:8a:da:90:ba:ed:3a:47:0c:05:a3:f7:a8:0a:8d:78 (RSA)
|   256 c2:64:ef:ab:b1:9a:1c:87:58:7c:4b:d5:0f:20:46:26 (ECDSA)
|_  256 5a:f2:62:92:11:8e:ad:8a:9b:23:82:2d:ad:53:bc:16 (ED25519)
80/tcp   open     http        Apache httpd 2.4.29 ((Ubuntu))
|_http-generator: WordPress 5.0
|_http-title: Billy Joel&#039;s IT Blog &#8211; The IT blog
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
|_http-server-header: Apache/2.4.29 (Ubuntu)
139/tcp  open     netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open     netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
9898/tcp filtered monkeycom
Service Info: Host: BLOG; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb2-time: 
|   date: 2024-02-02T01:23:44
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: BLOG, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: blog
|   NetBIOS computer name: BLOG\x00
|   Domain name: \x00
|   FQDN: blog
|_  System time: 2024-02-02T01:23:44+00:00
```

Running enum4linux did find some shares

```
        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP              BLOG ```
//10.10.218.153/BillySMB        Mapping: OK Listing: OK Writing: N/A
```

In the shares found 2 pictures and a video file jpeg file had a txt file hidden in it saying it was a rabbit hole

The webhost is running wordpress going to start a scan with wpscan did not find much 2 users

```
[+] bjoel
 | Found By: Wp Json Api (Aggressive Detection)
 |  - http://10.10.218.153/wp-json/wp/v2/users/?per_page=100&page=1
 | Confirmed By:
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[+] kwheel
 | Found By: Wp Json Api (Aggressive Detection)
 |  - http://10.10.218.153/wp-json/wp/v2/users/?per_page=100&page=1
 | Confirmed By:
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

Cracked a user for the wordpress site
```
`kwheel:<Redacted>`

Found a possiable exploit for wordpress 5.0 its running Twenty Twenty theme could nver get this running so I just went to metasploit and got a shell from the exploit on there
https://github.com/v0lck3r/CVE-2019-8943?tab=readme-ov-file

```
define('DB_NAME', 'blog');
define('DB_USER', 'wordpressuser');
define('DB_PASSWORD', 'LittleYellowLamp90!@');
define('DB_HOST', 'localhost');
```

Just used PwnKit to get root
