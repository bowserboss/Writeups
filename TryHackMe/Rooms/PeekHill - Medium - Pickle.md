Started a nmap scan
```
20 ftp-data
21 vsftpd 3.0.3 anon login allowed
22 OpenSSH 7.2p2
7321 ncat into this
```
Starting with the FTP since it has anonymous login and I found 2 files one called `.creds` and one `text.txt` the test file just says vsftpd test file and the creds file if you load it in cyber chef and it looks like python pickle so make a pickle script to read the output and then sort all the `ssh_pass` and if you sort it right you will get a login to SSH `gherkin:p1ckl3s_@11_.` now we have access to the user gherkin he has a pyc file in his folder so were going to transfer the file to are host with scp
`scp gherkin@10.10.59.161:/home/gherkin/cmd_service.pyc` 
and were going to use `uncompyle6` to decode the script and we can open are python3 IDE and import the crypto package and then enter in the `long_to_bytes` one will be username and the other will be password and we got the login now for dill `dill:n3v3.....` but not his ssh account but we got that service running we can connect using ncat and ask for a username and password and now we have access to dill ssh account but with very limited access but we can read the .ssh folder and get there `id_rsa` key and it has no password on it so we can ssh into dill account and get a good shell when running `sudo -l` we can run a program called `peak_hill_farm` and it want a base64 encoded input but it also as to be pickled as well and we can use this to read the root flag 