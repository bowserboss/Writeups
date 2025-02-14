Started with a nmap scan
```
21 vsftpd 3.0.3
22 OpenSSH 7.2p2 
80 Werkzueg 0.9.6 Python 2.7.9
```
Going to the ftp it has anonymous login and has 2 files called `notice.txt,password` the note says that they made a temp password app and looking at the password file this is the app but we need a employee id so we will come back to this later
Going to the website we hey a `odoo` instance and its asking for a email and password 
```
/logo
/web
```
was nothing finding much found a way to back up the database but we need a login for this so going back to the binary going to open it in gharia and when we look at the function of the password we get `971234596` and gives us a password of `SecurePassword123!` and we can backup the database to a zip file and when we unzip it we get a file store and a sql file greping the sql file we get a email `admin@antisoft.thm` we can also grep and see its running `oddo` version `10` and its vulnerable to `cve-2017-10803` we need to install the plugin that it says and use the python code given and make a pickle exploit after the plugin is installed then go to settings and and click on anonymize database and then click at the top above the first txt box and we can upload a file had to base64 encode the payload and finally got a reverse shell as `odoo` and looking at the file system we are in  a docker container and on the root folder we have a program called `ret` got it onto my host and when we run it on the other host it says `exploit me to get onto the box` so time to reverse now going to open it in `gdb` we start it with `r` and if we enter a bunch of `A` we get a segfualt next we make a pattern to get the location of `$rsp` and getting the offset of `136` until we reach the `$rsp` and then we vaildate it with 136 A's and 8 B's and then the `$rsp` is filled with only `8 B's` so now we can make a python script then we put the payload on the box and run it like this `(cat p.p;cat) | ./ret` we need to put chisel on the box after we go that we can setup a tunnel and get a connection and then run a script I had made to exploit the ret binary and now we are `zeeshan` and we can go to `.ssh` and get the `id_rsa` key and ssh into the host with a real shell and looking around there is a `/exploit_me` looks like we need to exploit this and we can write another script to exploit this rop to get root 

