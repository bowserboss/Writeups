Started with a nmap scan
```
22 OpenSSH 7.2p2
80 Apache 2.4.18
2222 telnet
9090 Tornado 6.0.3
```
Starting with a gobuster scan
```
/index.html
```
Opening port `2222` with ncat we get a login screen and we can make a account and there is a option to see the secret folder and it gives us port `9090` so we need to do a buffer overflow so we can leak the real secret folder and we got it `/40b5dffec4e....................../` and its for the webhost on port `9090` looking at the source there is a `?hackme=` and its vulnerable to SSTI with this payload `{{7*7}}` gives us `49` we know its also running the tornado web framework and there is a SSTI for this framework with this payload we can get the username running this webapp `{%  import os %}{{ os.popen("whoami").read() }}` and we get `zeldris` as the username and we can run any commands we want now on the host and can make a sh script for a rev shell and we get a call back on the host now and were `zeldris` we can run pip install as root so we can go to GTFObins and get root  