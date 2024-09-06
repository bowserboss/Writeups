Started with a nmap scan
```
22 OpenSSH 7.6p1 Ubuntu
80 Apache 2.4.29
3306 MYSQL 5.5.5-10.1.48-MariaDB
```
Starting with the webhost looking at the code for the main page there is a domain we need to add to etc hosts `empline.thm` and it also has a sub domain with it so ima scan for all subdomains did not find anymore other than `job` so I looked at job and its a forum were we can apply for a job and the import resume were can do xxe injection with a `docx` file I read the `/etc/passwd` there is a article online on how to do this and looks like there is only 2 users `ubuntu` and `george` with this I was able to read the `config.php` file and got some database creds for the user `james` password `ng6pU.....` after looking in the database I found the users and there were 3 users `admin, george, james` and `george` password cracked to `pretonn......` and this can be used to login into his SSH account when I was running linpeas I noticed we have `cap_chwon` with ruby and we can use this to modify root files like shadow 
`ruby -e 'requir "fileutils"; FileUtils.chown(1002, 1002, "/etc/shadow")'` 
when doing this exploit we need to make sure that we set the group id right and the user id 
now we can read the shadow file and write to it so we can replace the root password with are own and then just `su root` and now we are root user 