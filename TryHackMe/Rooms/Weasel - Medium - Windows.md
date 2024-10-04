Started with a nmap scan
```
22 OpenSSH for Windows 7.7
135 RPC
139 netbios=ssn
445 microsoft-ds
3389 RDP
8888 Tornado httpd 6.0.3
```
 Going to the webhost its `Jupyter Notebook` and its running version `6.0.3` going to the SMB had to do some guessing and we got a guest account with no password and we got some shares we can read and one we can write to once on the smb we there is a bunch of files and some folders there is a `misc` folder it has a txt file with a token in it and we can access the notebook now and it has the same files as the smb but what we can do is just make a new file and put  a python reverse shell in it and run it and we get a call back as `dev-datasci` and in the home folder there is a `id_rsa` key for a user `dev-datasci-lowpriv` and now we can ssh into the host and now were in a wsl console and now we can download winPEAS and scan the host I ended switching my shell to metasploit and run local exploit suggester and we can use `always_install_elevated` but did not work since we are a session 0 we need to get to session 1 pid and then we can re run the exploit agin and now we are system admin 