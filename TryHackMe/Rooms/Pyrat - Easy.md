Started with a nmap scan
```
22 OpenSSH 8.2p1
8000 python 3.11.2
```
When going to port `8000` on the web browser it says try a more basic connection so I used ncat to connect to it and when I tried to try something its says EOF when reading a line and from looking at the nmap scan its a python web server running on its default port and I started trying some python imports like `os` and it did not return anything so I just tried a reverse shell in python and it worked got a call back as `www-data` also doing a search on google for pyrat I found a github with the script and we can do some commands to get shell right away like we can type shell and get `www-data` if we type admin we get a password prompt but the hard coded pass does not work so we can maybe try brute forceing it I had chatGPT make me  a script to try and find the password and we got the password `.......` and we are now root 