Started with a nmap scan
```
22/tcp    open  ssh     syn-ack ttl 63 OpenSSH 6.7p1 Debian 5+deb8u3 (protocol 2.0)
53/tcp    open  domain  syn-ack ttl 63 dnsmasq 2.76
80/tcp    open  http    syn-ack ttl 63 lighttpd 1.4.35
1897/tcp  open  upnp    syn-ack ttl 63 Platinum UPnP 1.0.5.13 (UPnP/1.0 DLNADOC/1.50)
32400/tcp open  http    syn-ack ttl 63 Plex Media Server httpd
32469/tcp open  upnp    syn-ack ttl 63 Platinum UPnP 1.0.5.13 (UPnP/1.0 DLNADOC/1.50)
```
doing a gobuster scan of port `80` since the website is just a blank page 
```
/admin
/versions
```
If we go to `/admin/` its **Pi-hole Version** v3.1.4 if we go to port `32400` its a plex media server and we can make a account after digging around the websites for a while I just tried the default login for a raspberry pi account and I could SSH into the host with these creds `pip:raspberry` now we have to search around for the root flag and then we find a `dammit.txt` file says the flag was delated if we run `mount | grep usb` we can see were its mounted and if we run strings on the `/dev/sdb` we can get the root flag 