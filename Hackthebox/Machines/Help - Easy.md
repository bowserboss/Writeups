Started with a nmap scan
```
22 OpenSSH 7.2p2
80 Apache httpd 2.4.18
3000 express.js
```
Nmap showed there is a domain linked to the website `http://help.htb/` doing a subdomain scan did not find any subdomains the main webpage is just a default apache page  
Starting a gobuster scan
```
/support 
/index.html
/javascript
```
If we go to support `HelpDeskz` is running doing a google search I found a github for it and there is a `README.md` file we can check for the version and its `1.0.2` which has a vulnerability in it for `Unauthenticated Shell Upload` but I cant get it to work but there is another vuln for authenticated sql injection if we go to port `3000` it says we need to query to find the creds if we send this request it shows there is some api running in the background `curl -s -G http://help.htb:3000/graphql --data-urlencode "query={Shiv}" | jq` now looking at the docs some more if we run this command we can get the password `curl -s -G http://10.10.10.121:3000/graphql --data-urlencode "query={ user { password } }" | jq` and we get this hash `5d3c93182bb20f07b994a7f617e99cff` and it cracks to `.........` and we can assume the username is `Shiv` but we still cant login so I need to figure out the username for the site I found another field we can query `curl -s -G http://10.10.10.121:3000/graphql --data-urlencode "query={ user { username } }" | jq` gives us `helpme@helpme.com` and now we can login in  if we make a ticket with a attachment and add `admin 1=1 -- -` after we right click on the attachment we see there is a sql exploit and we can user this to get the admin account and when we so we get a hash `d318f44739dced66793b1a603028133a76ae680e` which cracks to `........` now we need the email `support@mysite.com` but we cant login we can try ssh and guess the username and I got it with the user `help` running linpeas did not find anything so I started to see if there is any kernel exploits for this os and I found one https://www.exploit-db.com/exploits/44298 running this now we are root 