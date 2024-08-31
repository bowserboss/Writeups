Started with a nmap scan
```
21
22 OpenSSH 7.6p1 Ubuntu
23
25
80 Apache 2.4.29
110
443
8873 closed
1313
2600
2605
4559
5151
5555
5674
5675
9098
10081
11201
15345
27374
57000
60177
```
This box has a lot of random ports to try and throw you off 
Ran a gobuster and only thing I found was a `/inferno` which goes to a login page that sends a GET request and has a auth header that hashes what ever we send in base64 after not finding anything to go off of I made a list of users and ran hydra on the login page and got some creds `admin:da...` and after we login were taken to a IDE its `codiad` its running `2.8.4` to get a reverse shell in the project we need to go to `themes/defualt/filemanager/images/codiad/mainfest/files/codiad/exmaple/INF/`
and right click and we can upload a reverse shell in this folder and then navigate to this path in the web browser and we get a call back from are shell as `www-data` looking into some files there is a `.download.dat` file in `dante` downloads folder that's encoded in hex and once you decode it has there creds for ssh in it `dante:V1rg1.....` once we got onto `dante` he can run tee as sudo and we can exploit this to drop a user in passwd as root or we can use PwnKit to get root