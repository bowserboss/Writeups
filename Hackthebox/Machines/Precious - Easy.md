Started with a nmap scan
```
22 OpenSSH 8.4p1
80 nginx 1.18.0
```
Nmap shows there is a domain connected to the site `http://precious.htb/` starting with a subdomain scan did not find any subdomains going to the website its a program that converts a webpage to a PDF when looking at the headers of the site its running `phusion passenger 6.0.15` doing some research about this its running pdfkit and there is a exploit for this `CVE-2022-25765` and we can use this to get a reverse shell and now we have access as the user `ruby` looking in a config file I found `/home/ruby/.bundle/config` has the creds for henry `Q3c1AqGHtoI0aXAYFH` and when we do `sudo -l` there is a script we can run as root `/opt/update_dependencies.rb` and we can control the path of the dependencies file and payload of all things have some scripts we can use and get the root flag if we run the script from the tmp folder 