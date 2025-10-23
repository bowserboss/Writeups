Started with a nmap scan
```
22 OpenSSH 7.9p1
80 Apache Tomcat
8080 Apache Tomcat
```
Going to the webhost there both the same site and its running tomcat version `8.5.54` and now we giong to run a gobuster scan
```
/manager/
/Monitoring/
```
And if we go to the monitoring we get a different site and its using `apache struts` so its vulnerable to `cve-2017-5638` and its running as `tomcat` trying to get  a reverse shell but we cant there some firewall or something blocking us but we can get the mysql creds from the `db_connect` file `admin:admin` and we can run mysql commands from the exploit and see what we get `python2 struts-pwn.py -u http://10.10.10.64/Monitoring/example/Welcome.action -c 'mysql -u admin -padmin -e "show databases;"'` and we get `users` back now we can run the same command `use users;show tables;` and we get the `accounts` table `use users;select * from accounts;` and we get a username and password `richard:9tc*rhKuG5TyXvUJOrE^5CK7k` and we can use this to SSH into the machine when running `sudo -l` we have control over a `test.py` we can run as root we can make a `hashlib.py` and the script will just use it from are home folder and we can spawn a pty and root 