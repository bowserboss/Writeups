Started with a nmap scan
```
22 OpenSSH 7.6p1
80 nginx 1.14.0
```
Going to the site we have a registration and a login we can make a account and then login and we have `1` gold there is a item that we can buy for `10k` and we can give gold as well but taking a step back going to test  the everything on the site with out logging first so running a gobuster scan on the host
```
/images
/login.html
/home.html
/index.html
/create.html
/giveing.html
/Purchase.html
/api
```
looking at the hint says look at the name of the site which is `"Race" Track` which leads me to think were looking for a race condition exploit so we can test the giving gold option we need to make 2 accounts and then send gold to one of them so to test this I get the request in burp and send it to the repeater tab and then make a group and make 50 dup tabs and send the request in the `send in group parallel` and when I log into the account I sent it we get 3 gold at first so some of the connections went threw so now we need to keep doing this until we hit the 10k gold coins after we hit 10k we can buy the premium account and its a calculator and testing basic ssti and we got it with `{7*7}` and we know the site is running express and `nodejs` using the require() statement we can get a reverse shell as the `brian` user going to run linpeas on the host found there is a binary called `manageaccounts` but we don't need to exploit this if you run pspy we can see there is a cron running a cleanup script we can change the name of this and make are own to read the root flag 