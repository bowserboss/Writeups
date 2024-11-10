Started with a nmap scan
```
22 OpenSSH 7.6p1
80 Node.js
```
Started with a gobuster scan
```
signup.html
login.html
index.html
admin.html
super-secret-admin-pannel.html
```
Looking at the source code of the `admin.html` its checking for the username `connor` for the login going to the secret admin page we can exacute python code but it has a blacklist of some words like `import` `eval` `exec` `system` we can use this command to view the file system 
`print(__builtins__.__dict__['__import__']('os').getcwd())`
after lots of attempts to get a reverse shell I finally got one and we are root but we cant do much looking around the system we are in a container so we need some creds for a better shell going back to that hash in the login page wrote a script to decode the password and its `........` and we can use these creds to SSH into the server looking around the host agin not much going on we cant sudo or anything so I ran linpeas and its vulnerable to PwnKit 