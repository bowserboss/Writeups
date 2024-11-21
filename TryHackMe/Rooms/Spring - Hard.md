Started with a nmap scan
```
22 OPenSSH 7.6p1
80 http
443 https
```
When you go to the webapp port `80` redirects to https and when I curl the web page I get a domain from the SSL cert `spring.thm` now trying to enumerate the spring boot framework I found some stuff
```
/actuator/
/sources/
/sources/new/
/sources/new/.git/
```
We found a `.git` folder so now we need to use the `gittools` to dump it looking at the git dump we found some username and passwords and its running `springboot 2.3.1` and there is a parameter its getting called `/?name=` and then it reflects the value still looking at the code we need to make a request with `x-9ad42dea0356cb04` and its looking for a ip in the range of `172.16.0.1/24` so if we make a request with this info and get back the `actuator` info there is a blog post on how to exploit this with `h2` database and we can access the `restart` and the `env` we can use a bash rev shell in a file took me a bit to get this working but I got a call back as `nobody` the passwords we found do not work for `johnsmith` so there is no way to get to john so we need to somehow bruteforce his password so I greped rockyou for strong passwords and we had found 3 passwords all similar just one word changed so I piped the word we greped for and  used `sucrack` to guess his password found the password for the use johnsmith `.......................` since there is ssh on the host I made a `authorized_keys` file and added my key in there now we can ssh and get a real shell and used pwnkit to get root 