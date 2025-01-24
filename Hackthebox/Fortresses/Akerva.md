Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Apache 2.4.29
5000 Python BaseHTTPServer
--------- UDP ------
161 snmp
```
Looking at the site on port `80` its a wordpress site going to run wpscan on the host found a user called `aas` and the site is a static site pretty much ran a gobuster scan did not find much besides normal wordpress files 
Going to port `5000` its a login page so we don't have much going back and re doing the nmap scan for udp and see if there is anything else and we did find a `snmp` server running with UDP when running `snmp_enum` in the processes there two interesting files `/dev/space_dev.py` and `backup_every_17minutes.sh` when going back to the webserver trying to look at the files on the SH file we get a internal server error so I changed to a POST request and got the file and there is also a flag in there from looking at the script it runs every 17 minutes and saves it to `/backups` and its saved as `backup_TIMESTAMP` so we can make a list of numbers to match and then FUZZ the folder and get the zip file  so when we curl the site we get the time of the server and we can use a unix converter and then make a list of numbers to `0000-9999` and then fuzz for the zip file
`ffuf -u http://10.13.37.11/backup_2025012320FUZZ.zip -w numers.txt` and now we have the zip file now we have a backup of the whole webserver I found another flag in the py file and its also the login for the webserver running on port `5000` `aas:FLAG` running a gobuster on this webserver
```
/console
/file
/download
```
When going to `/file` we get a error so lets fuzz it and see if we get any parameters but it not get anything looking back at the source it gives us the parameter `filename`  and I tested it for LFI and we got a hit `http://10.13.37.11:5000/file?filename=../../../../etc/passwd` and we see the user `aas`and now since we have a LFI we can exploit the `/console` you can google werkzueg console bypass and I got the pin `254-330-006` this pin will change if the host is restarted and make sure to use the python 2 script now we can get a reverse shell from the console and we get a call back as `aas` after we stabilize the shell in the home folder there is another flag in the file `.hiddenflag.txt` now that we are on the host machine I ran linpeas and we can exploit `CVE-2019-18634` its a sudo exploit if we compile it and send it to the machine we can run it but we have ot make sure we have the same packages in the root folder there is a `secret_message.md` and its base64 encoded and then vigenere cipher but there is some letters missing so I made a script to find the missing letters  and I found the key to be `ILOVESPACE` and now we have the final flag 

Flags
1. First flag is looking at the source code on port `80` 
2. when we run `snmp_enum` in the process we find a flag
3. Send POST request to SH file
4. in the zip folder look at `space_dev.py` file
5. Use the LFI to read the flag in the home folder of `aas` 
6. need to exploit the console for reverse shell and flag is in the home folder 
7. root folder 
8. `secret_message.md` in root folder 