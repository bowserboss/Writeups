Started with a nmap scan
```
8080 Python 3.6.9 Wekzeug 1.0.1
```
Starting a nikto scan did not find anything
Fuzzing for subdomains did not find anything
Starting a gobuster scan
```
/login
/register
/logout
/shutdown
/index
```

There is a webhost running on port `8080` that goes to a login page and we can make are own user account since we know the application is running a template and is running python we can try some python SSTI  `Jinja2` after we login in we can try SSTI in the `index` page and the payload `{{7*7}}` gives us a 404 sorry error but it also output `49` for us so it did exacute the payload 
`http://vulnnet.thm:8080/{{config}}` 
to dump all the config variables the secret key is `S#perS3c.....` 
And there is also a filter in place to try and filter out the SSTI  lots of stuff is blocked after testing for a while I came across a bypass and had to encode some parts of it in HEX and now we have a reverse shell call back to us 
```
{{request|attr('appliocation')|attr('/x5f/x5fglobals\x5f\x5f')|attr('/x5f/x5fbuilitins\x5f\x5f')|attr('/x5f/x5fimport\x5f\x5f')('os')|attr('popen')('hexpayload')|attr('read')()}}
```
And we are the user web got root with PwnKit