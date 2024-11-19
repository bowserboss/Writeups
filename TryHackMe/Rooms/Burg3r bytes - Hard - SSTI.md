Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Wekzueg 3.0.2 Python 3.8.10
```
Starting with the webapp looking around we can add stuff to the cart we cant make a account and the login page does not work the only part of the site that seems to work is the checkout function of the site 
Starting a gobuster scan
```
/login
/register
/basket
/checkout
/console
/receipt
```
In order to see if we can exploit the console we need to find a LFI spend a bunch of time testing to sqli and LFI and did not find anything so going back to the checkout we cant buy anything with are credit but we have a section were we can apply a discount code the room did say something about the 3 million visitor gets everything for free there is a item called `tryhack3m` and this works as a discount code but this only applies a 50% discount and we still cant buy anything if we open two web browsers and send to request we can apply it twice and we get a receipt for are purchase and we have a new parameter called `name=` and we can see its reflecting a value on the site in bold letters we can test this for SSTI running `{{7*7}}` and we get `49` back so we do have SSTI and when I get it to error out we find its a `jinja2` webapp with this payload we can read the `/etc/passwd` file `{{ request.__clases__.__load_form_data.__globals__.__builtins__.open( /etc/passwd ).read }}` so now we can change it a little bit so we can exacute commands `{{ config.__class__.from_envvar.__gloabals__.__builtins__.__import__("os").popen("ls").read() }}` if we base64 encode are bash revshell we can get a call back to are machine as `root` but were in a docker environment but we cant download files there is no curl or wget but if we base64 encode it and pipe it to a file we can run linpeas this shows we have a script running in a cronjob looking at this script it can access the main host with the `site.db` file so if we copy the script to a new one we can modify the script so we can get the `server.crt` file we are missing with this file we can add a SSH key to the authorized_keys file and then we can modify the file again so we can add are keys to the main host now we need to modify the script again and base64 encode it and run it again now we added are keys to the main host and SSH into the host now 