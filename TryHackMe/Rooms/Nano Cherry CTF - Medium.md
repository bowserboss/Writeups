It gives us a login to a backdoor account `notus:dontbeascriptkiddie` and gives us a domain as well `cherryontop.thm` 
Starting a nmap scan to see if there any other services running
```
22 OpenSSH 8.9p1
80 Apache 2.4.52
```
Running a subdomain scan only found one `nano` 
Starting a gobuster scan on the main web host 
```
/content.php
/images
/index.php
/css
/js
```
The main site is pretty much a static site with one php page 
The nano site there is a admin login page 
So now moving on to the creds it gave us works for ssh there is a cronjob running as bob-boba and we can edit the `/etc/hosts` file so we can add this domain and host it are self `cherryontop.tld` and we can host a revshell as the file name and location that the curl is doing and then it exacutes in bash and now we are user `bob-boba` Going back to the web app I'm going to try and enumerate the username for the admin panel and we did find a username called `pup...` and now we can try and crack the password and it is `mas...` now that were loged in on the main page there is a person called Jax and if you read his bio he gives a password and says its for ssh and its the password for the user `molly-milk` `ChadCher..............` now were back at a dead end we have no way of privsec from this point but there still one user we need to get access to so back to the web app on the main site there is a content page that uses guest level access maybe we can see if there is more than 4 facts so I made a list up to 10000 and going to use ffuf and see if there is any other ids there is other ids `43,50,64,20` but did not hint at anything so we got access to all user but 2 so the request is using guest and is base32 encoded what if we change it to a encoded version on `sam-sprinkles` and on id `43` we got login creds for his SSH account `SammyI......` now we got all three parts of `chad-cherry` password `n4n0ch3rry.........` now were on the last user there is a note saying the `rootPassword.wav` file has the password for root after doing some research online I found a program called `sstv` useing this it takes the jpg image out of the wav file and if you open the image it says `Nano...................` and this is the root password 