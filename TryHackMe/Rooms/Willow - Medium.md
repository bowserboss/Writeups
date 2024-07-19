Started with a nmap scan
```
22 OpenSSH 6.7p1
80 Apache 2.4.10
111 NFS
```
Starting with the web page its some encoded numbers its hex and the message decoded says `hey willow here your ssh private key -- you know where the decryption key is`
Going to the NFS file share we can run `showmount -e 10.10.62.15` and we get a path `/var/failsafe/*` lets mount this path `mount -t nfs 10.10.62.15:/var/failsafe /tmp/nfs -o nolock` and then we go to the mount point and look into the folder and found a file called `rsa_keys` shows us the numbers for the public and private key pair 
```
Public 23,37627
Private 61527,37627
```
so we go to a RSA calculator and input the hash and keys and now we have the unencrypted RSA private key now going to send it to ssh2john and then use john to crack it with `rockyou` and got the password for the key 
`wildf.....r` 
now to ssh into the server `ssh willow@10.10.62.15 -i id_rsa -o PubkeyAcceptedKeyTypes=ssh-rsa` I think the box is just really old and ssh changed there algo to log in now we got user access as willow to get the user flag it seems to be blocking common ports on the host but not port 1234 so we got the user.jpg which is the flag and running sudo -l we can run `/bin/mount /dev/hidden_backup /tmp/` and we got a file called creds with root creds in it then flag.txt says it gave us the flag along time ago so going back to the picture and using `steghide` and the root password now we got the root flag
`steghide extract -sf user.jpg`