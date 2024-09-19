Started with a nmap scan
```
22 OPenSH
10000 ?
```
 When connecting to port `10000` with ncat it ask us how many exploits to send and then pings tryhackme.com we know that what's running on this port is a python script so maybe if we inject some python code into the txt area we can run code if we do `eval('1')` it send just one ping request and also if we do this we can read were the web app is located at `eval('__import__("os").system("pwd")` and we are located in `/home/king` so we can run the same thing agin and this time run a revshell and we get a call back as `king` running the command cronjob we can see there is 2 files one running as king and one running as root we can modify the root.sh file and echo in  a reverse shell and wait for the cronjob to run and we are root now