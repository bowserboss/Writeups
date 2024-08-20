Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
```
Doing a nitko scan found
```
/index.php
```
Doing a gobuster scan
```
/news.php
/index.php
/register.php
/detail.php
/note.txt
/~files/pass.txt
```
We can go to register page and make a account and under the news section that has a note that says `its about LFI or is it RCE or something else` 
If you go to the detail page at the bottom in the code it says try to use `page` as get pram in the note txt file it gives us a path to a pass.txt file and then did some more dir searching and found a files folder with a pass.txt file and says `admin__admin mans two numbers are there` and should be enofh to get pass so we generate a list of number from 00-99 and then I had ChatGPT help me with a python script to brute the login and you have to wait 60 seconds so you don't get blocked this takes some times but I got the password `admin..admin` and now we can use that LFI we found
`http://sfaezone.thm/detail.php?page=/etc/passwd` and look at the source of the page and we can see the passed file so we can read the access log file for apache and we can inject a php shell into the webserver I injected a cmd shell into the user-agent into the access log and then I put `&cmd=id` and got code to exacute as `www-data` so we can get a rev shell using mkfifo and URL encode it looking around the sever I went to the files home folder and there is a file called `.some#fake_canbehere` and its a hash for the user `files` and we have ssh access now after a cracked it with hashcat `magic` now you can get root with PwnKit but there is also a port 8000 open on the machine you can forward with socat and exploit it from the webserver and get into the use `yash` like that