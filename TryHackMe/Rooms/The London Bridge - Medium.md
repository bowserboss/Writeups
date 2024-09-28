Started with a nmap scan 
```
22 OpenSSH 7.6p1 Ubuntu
8080 gunicorn
```
Looking at the web app we got 2 pages so far gallery and contact on the gallery we got a picture upload there is also a note on the gallery page saying to the devs to make sure people can also use links to upload I'm going to start a gobuster now and see if there is anything else 
```
/contact ###Post to this goes to feedback
/feedback 405
/gallery ###Post to this goes to upload
/upload
/dejaview   ## url image upload 
/view_image
```
This `dejaview` page has a input that calls back to us we need to find a hidden parameter from the hint and found this parameter  `www` and when you go to it get a error so we need a SSRF bypass which this one worked and then we need to port scan to find the open port `http://0:80` and then with the wordlists from seclists `big.txt` we found this path `/.ssh/id_rsa` and we can also get to the `authorized_keys` file and find the username is `beth` so now we can ssh into beth after running linpeas it says its vulnerable to `CVE-2018-18955` but were going to need some extra files there is a kernel exploit github with these files `exploit.dbus.sh rootshell.c subshell.c subuid_shell.c` and then run the sh script and now we are root now we can get the root flag and now for the Charles flag if we go to this home folder and there is a `.mozilla` folder maybe we can dump some creds so we tar the firefox folder and get it to are machine and use a program called firefox_decrypt and we need to `sudo chmod -R 7777 firefox` and now we can dump any creds that are in the browser and we get a password of `theking.......` 