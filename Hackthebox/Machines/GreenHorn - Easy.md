Started with a nmap scan
```
22 OpenSSH 8.9p1
80 nginx 1.18.0
3000 pop?
```
There is a domain that it redirects to `http://greenhorn.htb` 
On port `80` just normal site but is running `pluck` version `4.7.18` which has a RCE `CVE-2023-50564`
On port `3000` is a git service running `gitea` version `1.21.11` we can make a account and see the source code for the repo after little bit of looking at the exploit and looking at the repo I found a `pass.php` in the repo with a SHA-512 hash and it cracked to `........` which is the password for the pluck and we can upload are zip file and get a call back as `www-data` user and the password also works for the user `junior` we have a pdf file called `Using OpenVAS.pdf` and has a pixeled password in there we need to unpixel it so first we need to convert to a ppm file using `pdfimages` then we use a tool called `depix` to try and get clear text image from the pdf and we got the password now for root `............................` 