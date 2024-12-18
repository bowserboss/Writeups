Started a nmap scan
```
22 OpenSSH 8.2p1
80 Apache 2.4.41
```
Going to the webserver there is a domain linked to the server and a subdomain for latex when using the latex instance if we remove the file path from the URI and we see all the files in the web root and we can read the tex files we can see the source code and see how to can get LFI `$\listinputlisting{/etc/passwd}$` and with this payload we can view the passwd file we can read the apache config and we find two more subdomains and then we can also view the `.htpasswd` file and we got a user and hash and we cracked the hash `vdaisley:........` and now we can SSH into the machine there is `gnuplot` running as a root as a cronjob so we can make a plt file and put it in  the `/opt/gnuplot` folder useing `system("cat /root/root.txt > /tmp/root")` and wait for the cron to run agin 