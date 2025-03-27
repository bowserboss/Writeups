Started with a nmap scan
```
22 OpenSSH 8.2p1
5000 Werkzeug/3.0.3 Python 3.9.5
```
Started with a gobuster `2.3-big`
```
/login
/register
/upload
/logout
```
Going to the website we can login in and make account after we do that we get a dashboard that wants a cif file and gives us a example of one doing some googling there is a exploit with the CVE of `CVE-2024-23346` and it gives us a poc we modify the poc to a bash reverse shell and upload it and click on the view and now we have a call back as the user `app` after stabilizing the shell I'm looking around and found a database of some users called `database.db` we got 3 interesting users mixed with other people doing the challenge they are md5 hashes out of the 3 I got one to crack its was the user `rosa` with the password of `unicorniosrosados` and now we can ssh with this user we can not run `sudo -l` when we was the user `app` we could not connect to port `8080` now we can as the user `rosa` going to forward the port to are localhost we can do this with this command 
`ssh -L 5000:127.0.0.1:8080 rosa@10.10.11.38`
After looking at this web app for a little bit I found its running `aiohttp` and is vulnerable to a CVE `CVE-2024-23334` which is a LFI exploit and we can read any file root can read 
`http://127.0.0.1:5000/assets/../../../../../../../etc/shadow` 
and now we can just get the flag from root 