Started with a nmap scan
```
22 OpenSSH 8.9p1
80 Apache 2.4.52
4369 Erland Port Mapper Daemon
25672 unkown
```
The nmap scan shows a redirect on port `80` to a website `http://cloudsite.thm/` there is only one thing we can access at the moment from the internet doing a subdomain scan on the webhost the contact form does nothing going to the login page it leads us to another subdomain `storage.cloudsite.thm` when we make a account on the site and login says we have a inactive sub and get no new functions of the site but it does give us a jwt token the login page has a `/api/login` and sends a post request with json formatting but cant test sql injections on this page and same with the registration page so somehow we probley need to get a active account somehow I tried changing the jwt token doing a gobuster on the `storage` site
```
/api
/api/login
/api/register
/api/active
/api/inactive
/api/uploads   request methods GET, HEAD
/api/docs      403 code
/assets/
```
I also tried enumerating the erlang demon but did not find anything going back to the website looking at the register function when we make a account I'm gonna see if we can just make a account with a active subscription and if we add `"subscription":"active"` into the json field when we make a account we now have a active subscription when we login and get to see a upload forum and maybe we can check out the api docs now as well we still get access denied going to the upload function on the site when we look at the request there is a javascript being loaded called `custom_script_active.js` and uploads all the upload functions and we go upload from URL from `/api/store-url`  if we scroll down we can see it we can try to use SSRF to see if we can get to the `docs` and we still got access denied but what we do know from the header its running express and the default port is `3000` we can try this with the localhost and add the `http://` to the url and now we can access the docs file and the only function we can see that we don't all read know is `/api/fetch_messages_from_chatbot` when making a post request with a empty json format and we get its asking for a username when we input our username it says its still under development and when we try admin its the same message but I notice we get the username reflected back to us so we can try SSTI and with a polygot it worked and its running `jinja2` so now we can get RCE with a reverse shell and we get a call back as `azrael` and running linpeas we can find the erlcookie `/var/lib/rabvbitmq/.erlang.cookie` now we can enumerate the erlang we got the name `rabbit` and the hostname `forge` we need to add this to are hostname so we can auth to it we can use RabbitMQ to auth to it `sudo rabbitmqctl --erlang-cookie '' --node rabbit@forge status` and we can also list the users and we get `root` we can export the definitions and it also says the `root password is the SHA-256 hashed value of the rabbitMQ root users password` so we can take this hash and base64 decode it and then take the `e3d7ba85` out that's the 4 bit salt and use the rest of it to auth as the root user `su - root` now we are root 