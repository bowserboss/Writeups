Started with a nmap scan 
```
22 OpenSSH 7.6p1 Ubuntu
80 Apache 2.4.49
```

Starting a Nikto scan 

Starting a gobuster scan with list `2.3-medmium` 
```
/console
/console/css/
/console/securimage/
/console/config.php size 0
/console/dashboard.php
/console/functions.php size 0
/console/mfa.php
/console/index.phps
```
When looking at the main index.php file there is some javascript code that obfuscated with packer we can decode this it says `fred I turned on php file syntax highlighting for you to review jason` we can view a lot of the php files buy adding a `s` at the end of php since syntax highlighting is on
Looking at `functions.phps` we see another comment saying `fred lets talk about ways to make this more secure but still flexible` and looks like it hashing the password in MD5 
Going to `config.phps` gives us a hash in `hex` that outputs to `jason_test_account` this must be are username and going back to the functions php file we can see its looking for a password that's in md5 hash that ends with `001` we can make a wordlist and then hash it all and take the last 3 numbers  and look for the ones with `001` at the end in this case were going to use `abkr` and now we can login to the site but were stopped with the `mfa.php` so now we need to build a script to bruteforce this mfa code im going to do this in bash with curl
```
for i in {0000..9999}; do echo $i; curl -s -X POST --data "code=$i" 10.10.213.175/console/mfa.php --cookie "user=jason_test_account; pwd=abkr" | wc | grep -v "   23   95   1523"; if [ $? -eq 0 ]; then echo FOUND IT; break; fi; done
``` 
Now we have the mfa code `1577` and now we got access to the dashboard and we got a file browser that we can read the file system with and we have a file viewer to see the files looking around we found that `jason` has a `.ssh` folder with his `id_rsa` file and we send it to john to crack it 
`john id_rsa.hash --wordlist=rockyou.txt --format=ssh` and now we have a ssh login for the user jason 
`jason:1a2.....` 
I ran `sudo -l` and fred can restart the fail2ban service and we can abuse this to get root as a reverse shell so I got into the user fred by `sudo -u fred bash` now were fred and we can edit this file `/etc/fail2ban/action.d/iptables-multiport.conf` and edit the `actionban` and put a bash reverse shell or we can just do `chmod +s /bin/bash` and save the file then restart the fail2ban service and do 3 failed login attempts and now we got a reverse shell as root 