Started with a nmap scan
```
22 OpenSSH 7.2p2 Ubuntu
8001 Apache 2.4.18
```
starting a gobuster 
```
/index.html
/reg.php
/web 
/web/resources
/web/resources/infoseek
/web/resources/infoseek/configure
/web/resources/infoseek/configure/key
/zip
```
Looking at the main site don't look like much so went to the `/zip` and has 100 zip files in it we will come back to this go to `reg.php` and when you look at the code there looks like a blacklist in the code and takes a input tested the input with sqlmap and some LFI list with ffuf I found a `id_rsa` key hidden in a bunch of folders so going back to the code we found interesting when looking at it we need 9 `,` and these `$` example payload `$$,$$,$$,$$$,$$,$$,$$,$$,$$$` and we get a response back saying nice password `cimihan.................` and this password is for the key we found now we need to look at the `403.php` page and has a hint that we need to bypass this somehow I found if we put this at the end of the path `/..;//../` we get a note at the bottom of the page saying you bypassed it `imsau` now we have ssh access to this user and there is a python script in the `/opt` folder that calls a file in the `/tmp` folder so we can make the yml file and give bash root permissions and then we are root after a couple of minutes 