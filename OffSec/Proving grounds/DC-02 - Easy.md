Started with a nmap scan
```
80 Apache 2.4.10
7744 OpenSSH 6.7p1
``` 
The nmap scan shows there is a redirect to `http://dc-2/` when going to the site its a wordpress site running version `4.7.10` users found
```
admin
jerry
tom
```
Looking at the site there is a flag page that says are wordlist might not work says to login as one user and it hints at using a tool called `cewl` so we make a wordlist with this `cewl http://dc-2/ -w cewl-site` and now we have a wordlist and we cracked to logins 
```
jerry:adipiscing
tom:parturient
```
There is a note on the site once we login into the user `tom` that says there is a shortcut and we can SSH into the user `tom` but were in a `rbash` shell we need to try and break out of it once we have done this there is a note saying `su` into the user `jerry` and we can and when we run `sudo -l` as jerry we can use the `git` binary as root user we can go to GTFObins and run one of the sudo exploits for `git` and now we are root 
