Started with a nmap scan
```
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 97:af:61:44:10:89:b9:53:f0:80:3f:d7:19:b1:e2:9c (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDBjDFc+UtqNVYIrxJx+2Z9ZGi7LtoV6vkWkbALvRXmFzqStfJ3UM7TuOcZcPd82vk0gFVN2/wjA3LUlbUlr7oSlD15DdJkr/XjYrZLJnG4NCxcAnbB5CIRaWmrrdGy5pJ/KgKr4UEVGDK+oAgE7wbv++el2WeD1DF8gw+GIHhtjrK1s0nfyNGcmGOwx8crtHB4xLpopAxWDr2jzMFMdGcIzZMRVLbe+TsG/8O/GFgNXU1WqFYGe4xl+MCmomjh9mUspf1WP2SRZ7V0kndJJxtRBTw6V+NQ/7EJYJPMeugOtbputyZMH+jALhzxBs07JLbw8Bh9JX+ZJl/j6VcIDfFRXxB7ceSe/cp4UYWcLqN+AsoE7k+uMCV6vmXYPNC3g5xfMMrDfVmGmrPbop0oPZUB3kr8iz5CI/qM61WI07/MME1uyM352WZHAJmeBLPAOy05ZBY+DgpVElkr0vVa+3UyKsF1dC3Qm2jisx/qh3sGauv1R8oXGHvy0+oeMOlJN+k=
|   256 95:ed:65:8d:cd:08:2b:55:dd:17:51:31:1e:3e:18:12 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOL9rRkuTBwrdKEa+8VrwUjloHdmUdDR87hBOczK1zpwrsV/lXE1L/bYvDMUDVD0jE/aqMhekqNfBimt8aX53O0=
|   256 33:7b:c1:71:d3:33:0f:92:4e:83:5a:1f:52:02:93:5e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINM1K8Yufj5FJnBjvDzcr+32BQ9R/2lS/Mu33ExJwsci
80/tcp   open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: DUMB Docs
3000/tcp open  http    syn-ack ttl 63 Node.js (Express middleware)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: DUMB Docs
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
Going to the site both webhost look the same and we can download a zip file which is a backup of the site and there is also a `.git` folder   looking at the source code there is a `/logs` endpoint that has command injection in it its using the `git` command with out full paths we just need how to craft a JWT token if we do `git log` there is one commit that says they removed the `.env`  file if and we can see the token used to sign JWT tokens so now we can craft are own token and has to be in this format
```
{
  "sub": "1234567890",
  "name": "theadmin",
  "role": "admin",
  "iat": 1516239022
}
```
and now we can use this token to see if we can auth `curl -s http://10.10.11.120/api/priv -H "auth-token: (token)" | jq .` and we should get that we are the admin back and if we do then we can hit `/api/logs` and do the command injection after some testing and reading the source code agin we got it `/api/logs?file=git log --oneline;id` and url encode it and we got the user `dasith` back and we can use a base64 encoded `nc mkfifo` and now we have a reverse shell on the machine and we can make a .ssh folder and set the permissions so we can get a better shell running linpeas there is a c file in `/opt` and its owned by root so we have suid binary exploit playing around with this binary we can get it to read any folder we want if we run the binary and in the first option type `/root` and we get the contents of the folder so if we do this to the `/root/.viminfo` file and then if it ask us if we want to save the file we stop the process with `control z` and then do `pidof count` and then go to the `/proc/pid/fd/3` folder we get the contents of the file  and we can read the `3` file and we get a `id_rsa` key and now we can ssh as the root user 