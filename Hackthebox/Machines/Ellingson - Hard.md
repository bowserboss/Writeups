Started with a nmap scan
```
22 OpenSSH 7.6p1
80 nginx 1.14.0
```
Going to the site there not much going on but there is 3 articles if we change the number to something higher we get a error page if we go to page `0` it says to change passwords and list some common passwords `Love,Secret,Sex,God` if we go to page 4 we get the error page and its a `python3` web app and looking at these errors if we look to the right we see we can access a console and we can exacute python code and if we do whoami we get the user `hal` 
```
import  subprocess
proc = subprocess.check_output('whoami', shell=True );
print (proc.decode('utf-8'))
```
and we can get a reverse shell as `hal` now and in his user folder there is a `id_rsa` but we cant crack it but we can just add are own keys for easy root we can just run `cve-2021-4034` but doing this the right way going to run linpeas and found we can access the `shadow.bak` file so we can try cracking the hashes there is also a file called `garbage` that's owned by root and its a unknown `suid` binary but we cracked `margo` password `iamgod$08` now we can run the unknown binary we can do a lib call trace with `ltrace garbage` and enter a fake password now we got the real password `N3veRF@r1iSh3r3!` and we get a menu but we cant get much out of it so we need to get the binary on are machine and we need to print out 500 `A` and enter that as the password and we get a segmentation fault and now we need to open it in pdb and find the point so now we need to make a script to drop us into the root shell 