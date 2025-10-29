Started with a nmap scan
```
PORT    STATE SERVICE  REASON         VERSION
22/tcp  open  ssh      syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 4d:20:8a:b2:c2:8c:f5:3e:be:d2:e8:18:16:28:6e:8e (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDkj3wwSWqzkYHp9SbRMcsp8vHlgm5tTmUs0fgeuMCowimWCqCWdN358ha6zCdtC6kHBD9JjW+3puk65zr2xpd/Iq2w+UZzwVR070b3eMYn78xq+Xn6ZrJg25e5vH8+N23olPkHicT6tmYxPFp+pGo/FDZTsRkdkDWn4T2xzWLjdq4Ylq+RlXmQCmEsDtWvNSp3PG7JJaY5Nc+gFAd67OgkH5TVKyUWu2FYrBc4KEWvt7Bs52UftoUTjodRYbOevX+WlieLHXk86OR9WjlPk8z40qs1MckPJi926adEHjlvxdtq72nY25BhxAjmLIjck5nTNX+11a9i8KSNQ23Fjs4LiEOtlOozCFYy47+2NJzFi1iGj8J72r4EsEY+UMTLN9GW29Oz+10nLU1M+G6DQDKxoc1phz/D0GShJeQw8JhO0L+mI6AQKbn0pIo3r9/hLmZQkdXruJUn7U/7q7BDEjajVK3gPaskU/vPJRj3to8g+w+aX6IVSuVsJ6ya9x6XexE=
|   256 7b:0e:c7:5f:5a:4c:7a:11:7f:dd:58:5a:17:2f:cd:ea (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBF5my/tCLImcznAL+8z7XV5zgW5TMMIyf0ASrvxJ1mnfUYRSOGPKhT8vfnpuqAxdc5WjXQjehfiRGV6qUjoJ3I4=
|   256 a7:22:4e:45:19:8e:7d:3c:bc:df:6e:1d:6c:4f:41:56 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGxr2nNJEycZEgdIxL1zHLHfh+IBORxIXLX1ciHymxLO
80/tcp  open  http     syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-favicon: Unknown favicon MD5: 76362BB7970721417C5F484705E5045D
|_http-title:     Starter Website -  About 
| http-methods: 
|_  Supported Methods: GET OPTIONS HEAD
443/tcp open  ssl/http syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
| ssl-cert: Subject: commonName=passbolt.bolt.htb/organizationName=Internet Widgits Pty Ltd/stateOrProvinceName=Some-State/countryName=AU
| Issuer: commonName=passbolt.bolt.htb/organizationName=Internet Widgits Pty Ltd/stateOrProvinceName=Some-State/countryName=AU
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-02-24T19:11:23
| Not valid after:  2022-02-24T19:11:23
| MD5:   3ac3:4f7c:ee22:88de:7967:fe85:8c42:afc6
| SHA-1: c606:ca92:404f:2f04:6231:68be:c4c4:644f:e9ed:f132
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-title: Passbolt | Open source password manager for teams
|_Requested resource was /auth/login?redirect=%2F
|_http-favicon: Unknown favicon MD5: 82C6406C68D91356C9A729ED456EECF4
|_ssl-date: TLS randomness does not represent time
| http-methods: 
|_  Supported Methods: GET HEAD POST
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
There is a domain and subdomain leaked in the nmap scan
`http://bolt.htb` and `https://passbolt.bolt.htb` doing a subdomain scan `mail,demo` were also found
The website on port `80` has a login page and a download section to download a docker image to load in this docker `sudo docker load -i image.tar` now we need to get shell access to inside the docker we can also use a tool like `dive` to look at layer of the docker image and we can see there has been a DB file added and removed and we take take the hash id and tar it up and get the contents of the file `tar xvf image.tar (hash)` and then untar it and we have the original DB file and it has a hash that we can crack it with hashcat and now we have the admin login for something `admin:deadbolt` now we can log into the site on port `80` looking at more source code we find a invite code so now we can make a account on the other sites `XNSS-HSJW-3NGU-8XTJ` this account also works on `mail` subdomain there is also a email verification is required to edit your profile maybe this is were we can do SSTI and if we do this payload `{{7*7}}` and then check the mail and verify it we then we get another email back with the answer of `49` and we can confirm that it is in the `name` field and after some testing we got RCE and got `www-data` back `{{ request.application.__globals__.__builtins__.__import__('os').popen('id').read() }}` so now we can try and get a reverse shell we can make a reverse shell script and we get a call back as the `www-data` user I used `nc mkfifo` and just curled down the script and run it now we got access found some DB creds for the mail database but it don't get us anything new looking around in the roundcube folder found creds to that database now this did not get us anything new also looking around `/etc/passbolt` and there is a `app.php` file with creds to another database there was 2 users in the database but no password hashes but the DB password is the same for the `eddie` user and we can SSH into the account now from running linpeas when we was `www-data` user we could see the user eddie had mail which is uncommon and there is a password management server were we can upload are passwords looking more around the home folder there is a chrome folder with a extensions and one is for passbolt and if we grep it for `PRIVATE` we can find a pgp key we can try to crack it `000003.log` file has it and we can grep this file and run strings on it to get the keep and then clean it up and run `gpg2john` and crack the password and it cracked to `merrychristmas` we now if we go back to the passbolt site we can go to `/setup/recover/key/token` and to get the key we can look into the database for it since we have the creds and we can also get the token and then we can reset the account after we add eddie pgp key to it and we have the password and now were loged into the password manager and it has the root users password  