Starting with a nmap scan on the ip given 
```
22 OpenSSH 7.2p2 Ubuntu
53 DNS
80 nginx 1.10.3
2222 OpenSSH 8.2p1
5555 ? to connect ncat
7777 ? ncat to connect
9201 BaseHTTPServer 0.3 Python 2.7.12
```

Doing a dig command 
`dig @10.13.37.10 -x 10.13.37.10` and we got a domain to add to are hosts file `www.securewebinc.jet` 
Looking at the code for the site found a `secure.js` file that I had to decode and found a url path to a login page 
`/dirb_safe_dir_rf9EmCEIx/admin/` 
Sending the request to burp and trying some common login bypass techniques did not get anything other then a known username of `admin` so I sent the request to sqlmap and it found a injection in the username parameter and there is a interesting database `jetadmin` and there is a user table with only one user of admin and we got a `sha-256` hash and it cracked to `Hackthesysten200` after logging in the website is pretty static the only thing that works is the sending of a email and its using a function to replace the bad words out of the email when we have the request in burp we can modify the first field and get command injection `system('ls')` and now we can list all the files now time for reverse shell got one with `echo base64 | base64 -d | bash` and we get a call back as user `www-data` looking in the home folder there is a elf file called `leak` I turned it into base64 to decoded it back to a elf on my host when running the file its leaking the RSP address and the offset is `72` and we can have chatGPT help us with making a script to overflow this but we are behind a firewall so we need to use a program called `frp` download it to are system and to the other host also and then edit the `frpc.toml` change this file to are ip and port `46075` and then edit are proxychains config to port `46075` and then we run socat with this command 
`socat TCP-LISTEN:13145,reuseaddr,fork EXEC:/home/leak &`
and now we need to run the program on are system `./frps -c frps.toml` and then run it as `www-data` `./frpc -c frpc.toml` and now if when we run proxychains we can connect to the service we just made `proxychains ncat 192.168.122.100 13145` and we have to use the ip on the remote host `ens3` interface that's were I got the `192` ip from now if we run `proxychains python3 overflow.py` the script I made we connect as the user alex and have the next flag now we need to zip the alex folder there some files we need and I sent it with netcat to my system there is a file called `encrypted.txt` its XOR encrypted so using a decrypted we got the contents of the file and gave us another flag now looking at the ports agin we need to open `9300` to are host with socat the note also said there is vulnerable programs running on ports `5555,7777` to open the port 
`socat tcp-listen:8080,reuseaddr,fork tcp:localhost:9300 &` now we need to make a java file that dumps the `test` index and we get the flag now time to move to what's on the `port 5555` now we need to make a python script to overflow the program running on this port and once we do so we pop a shell just like before and now we are `member manager` and we have the flag in this user home folder we can go into `tony` home folder and there is a some files `key.bin.enc`, `keys`,`secret.enc` so we need to get these files to are host and then use a tool called `RSACTFtool` 
`python3 rsactftool.py --publickey ../public.enc --private` 
and now we need to run this comand
`openssl rsautl -decrypt -ssl -inkey privatye.key -in key.bin.enc -out export`
and we have the decoded txt now and the flag for this challenge now its time to overlfow the last binary on port `7777` and once we do a heap overflow bypassing canary we get a shell as `memo` and get the final flag 

Flags:
1. after you do to domain its on the main page `Digging in...`
2. found secret folder on the webhost view the source code for the flag `Going Deeper` 
3. after logging in another flag on the main page `Bypassing Authentication` 
4. after getting command injection there is a txt file called `a_flag_is_here.txt` `command` 
5. after overflowing the leak file we connect as alex and get the flag.txt `Overflown` 
6. alex home folder has a xor encrypted txt file with a flag `secret message` 
7. opened port `9300` and make a java program to dump the information `Elasticity`
8. need to overflow the `babyherap` process and get a shell as `membermanager` flag is in home folder `member manager`
9. In tony home folder there is some encoded files when you decode them we get the flag 
10. need to heap overflow and bypass canary on port `7777` to get the final flag 