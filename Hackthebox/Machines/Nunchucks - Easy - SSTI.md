Started with a nmap scan
```
22 OpenSSH 8.2p1
80 nginx 1.18.0
443 nginx 1.18.0
```
Nmap shows us there is a domain linked to the webhost `nunchucks.htb` port `80` also redirects to https we cant make a account or try to login in says login and registration is disabled doing a subdomain scan found only one `store` but its just a page that says the store is coming soon but here if we enter a email its reflects it back to us and knowing that we looking for a SSTI injection I found it here `{{7*7}}@t.com` and we get `49` back given the name of the room and looking at the framework I found one called `nunjucks` and after some testing I got commands to return back to me 
`{{ range.constructor('return global.process.mainModule.require(\"child_process\").execSync(\"id")')() }}@e.com` and we got the user `david` and we can use this to also get a reverse shell and `david` running linpeas shows us we can get root with PwnKit 