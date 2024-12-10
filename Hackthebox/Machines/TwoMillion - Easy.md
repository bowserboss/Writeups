Started with a nmap scan 
```
22 OpenSSH 8.9p1
80 nginx
```
looking at the site we have to make a invite code to join so looking in the code of the site we found this link we have to make a POST request to
	`curl -X POST 2million.htb/api/v1/invite/how/to/generate`
and this gives us a hint on how to make a invite code its encrypted in rot 13 and gives us the nest link to make the code
	`curl -X POST 2million.htb/api/v1/invite/generate`
and now we have a code
we need to get a list of the api routes for `/api/v1/admin` if you go to burp and send a get to `/api/v1`  you will get a list of all the routes there is 3 under the admin but 13 total routes we can make are self admin with this endpoint `/api/v1/admin/settings/update` now we can make are self admin and get to the admin endpoints now we need to find which endpoint has a command injection vulnerability `/api/v1/admin/user/generate` is vulnerable to the injection using this payload we can get the injection to work `ls #` and we are `www-data` now using this threw burp we found a  `.env` file which has are creds in it for the user `admin` we need to find the mail its in `/var/mail/admin` and now we finding the cve to get root which is `cve-2023-0386` and now we can get root for the extra credit we need to find the glibc version do it that we can `ldd --version` the cve for this exploit is `cve-2023-4911` the answer for the last question is `GLIBC_TUNABLES` 