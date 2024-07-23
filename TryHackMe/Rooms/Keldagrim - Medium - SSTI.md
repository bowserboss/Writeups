Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Werkzeug 1.0.1 Python 3.6.9
```
started a gobuster scan `2.3-medmium`
```
/services
/admin
/team
/wow
/runescape
```
So there is a `/admin` and looking at the cookies there is a `base64` cookie that decodes to `guest` so we make a base64 hash for admin and put it as are cookie and see can see the admin panel so playing around with the cookies the sales cookie can be exploited for SSRF we can run `{{7*7}}` and encode this to base64 and it output the math problem and from the nmap scan we know its python running in the backend so the template must be Jinja2 and we can read the `/etc/passwd` with the SSTI exploit we can get a reverse shell from the SSTI did some googling and found a payload now we got a callback on the reverse shell as `jed` got root with PwnKit but we could of also exploited the `LD_PRELOAD` with `/bin/ps`
