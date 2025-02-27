Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.6p1
80 Apache 2.4.29
443 Apache 2.4.29
31337  some binary
```
Starting with the ftp server it has anonymous login enabled and when we login there is 6 images running these images with stegseek there was nothing in them and we can also not upload anything to the FTP 
Going to the webserver on port `80` says there is nothing found 
Going to the webserver on port `443` we get a image and says `the magic word` im going to run `dirsearch` on port `443` did not find anything on port `80` but on `443` we found 
```
/candybar
```
when going to the `candybar` we get a message that is base32 encoded and then shift cipher and says `always check the SSL cer for clues` looking at the cert we get a domain called `adventure-time.com` and there is also another domain for the email `land-of-ooo.com` we will just try the first one for now if we use the other site `land of ooo` we get a different site scanned both sites for subdomains but did not find anything so going to run a gobuster scan on the `land of ooo` domain now
```
/yellowdog/
/yellowdog/bannastock/
```
now we found another folder called `bannastock` and it ask us if we changed the password and gives us some morse code with `/` and delimiters and when we decode it we get `THE BANNAS ARE THE BEST!!!` but we don't know what to do this with so lets do another gobuster scan  and we found another folder called `/princess/` and it has a message about changing the usernames and they will keep the new username `save` in the safe looking at the source code there is a secret text encrypted with `aes` in `cbc` mode and gives us everything to decrypt it we can use chatGPT to make a python script to decrypt it and we get a message saying `magic safe access on port 31337 magic word ricardio` now we have the new username `apple-guards` and now we can try the only login forum we could find which is SSH we find a file called `mbox` and its a email telling us he left a hidden file on the system searching the file system with his username we find a file in `/var/fonts` called `helper` and when we run the file it ask us if we solved the puzzle reversing this binary we find a system command be exacted to a file `/usr/share/misc/guard.sh` and the password for the puzzle is `Abadeer` and we got his password now `My friend Finn` and now we can su into the user `marceline` and there is a file called `I-got-a-secret.txt` and it has a binary code in the message but this got us no were looking at the hint says we need to research `cutlery` 
and we get `spoon programeing lnag` and we got the secret key now `ApplePie` now using nc to connect to the port `31337` and give it the new password we get the password for the `peppermint-butler` user `That Black Magic` and when we get access to the next user there is only a image `butler-1.jpg` so looking at the image we got a steg challenge now looking for other files owned by this user we find 2 more files 
```
/usr/share/xml/steg.txt
/etc/php/zip.txt
```
In the steg txt file is a password `ToKeepASecretSafe` using steghide and this password we get a `secrets.zip` and going back to the zip txt file we have another password `ThisIsReallySafe` and we can unzip the zip folder and get `secrets.txt` and it gives us a partial password `The Ice King s????` we can use crunch to make a password list and try to bruteforce the login for `gunter` and we got the password now `The Ice King sucks` now we can SSH into this user we don't own any interesting files so lets look at are perms and we have `exim4` perms we just need to see what port its running on and version we can read this file `exim4.conf` and its running on port `60000` and we can use `CVE-2019-10149` to get root access and the final flag is in the `bubblegum` home folder 