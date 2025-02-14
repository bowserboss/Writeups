Started with a nmap scan need to use `-Pn` box has no ping
```
22
80
```
Going to also start a gobuster scan with a `php` extension
```
/index.php
/images
/css
/js
/fonts
/robots.txt
/zYdHuAKjP
```
When we go to robots file it gives us a folder and when we go to the folder the webpage says access has not been granted so taking a guess and looking at the cookies if we change are cookie to granted we get access and we get something that looks like a user and password looking at the hint we need to make a python script to decode this and tells us how to decode it I had chatGPT help me took a little bit but we got a the login `magna::magnaisanelephant` and we get the first flag and there is a note for us saying spooky made a binary and there is `radar2` and `gdb` on the server and they want us to reverse it and we can go straight to root if we do this 