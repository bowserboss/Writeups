Started a nmap scan there so many ports open on a nmap scan so lets try something else I used ncat to try and connect to it since http was not working and it gave me a message saying the journey has just begin try printing banners from the first 100 ports so I used this command
`sudo nmap -p 1-100 -sV --script=banner 10.10.52.414` and we got some interesting ports but we are looking for the trollface told us what port to go to and I found 3 ports 
```
111 rpc
2049 nfs
3333 OpenSSH 7.6p1 
```
I mounted the rpc path and there was a `backup.zip` folder and got it to my machine and it was password protected so used zip2john and cracked the passowrd to `zxc....` I unzipped the folder and its a backup of `hades` home folder and has a `id_rsa` key in it and now we can ssh into the server but it drops us in some type of shell doing some searching its a interactive ruby shell to get out of the shell we run this code `exec "/bin/bash"` and now we in a bash shell now to get to root we have `cap_dac_read_search` abilities on tar so we cap grab the shadow file and then get the passwd file and then crack both accounts and we got root now
`root:........`
`vagrant:vag...` 