Started a nmap scan
```
22 OpenSSH 6.6.1p1
80 Apache 2.4.7
```
Started with a gobuster scan `2.3-medmium` doing a scan in the scripts
```
/scripts/
/scripts/ie/
/report
/admin.php
/register.php
/acc.php #onyl for admins
/fourms.php # only for admins
```
If you go to the webserver and go to report there is a binary file ELF there is a list of emails in the program and a guest account `guest:guest` I found a `admin.php` file with a login forum maybe we can take that list of emails could not crack any but we can also just make are own account we can make account with null bytes `%00` at the end of the admin account with email and now we can login as admin tested `acc.php` for sql injection no matter what I input says RCE detected on the `fourms.php` page we can get xxe injection and to read the passwd file
`<!DOCTYPE replace [<!ENTITY xxe SYSTEM "file:///etc/passwd>" ]>`
Now that we can read the passwd file lets see if we can read other files on the webhost like `acc.php` use this payload we can do so
`<!DOCTYPE replace [<!ENTITY xxe SYSTEM "file://filter/convert.base64-encode/resource=acc.php" ]>`
and when read this file there is some creds hidden in the file
`cyber:super#se......` 
and this can be used to login into the SSH account when we run `sudo -l` we have permission to run `run.py` but we can just rename the file and make are own with this code in it
```
imoport os
os.system('chmod 4777 /bin/bash')
```
and then run the file with sudo and then this command `/bin/bash -p` and were now root