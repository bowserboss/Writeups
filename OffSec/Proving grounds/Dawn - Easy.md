Started with a nmap scan
```
80 Apache 2.4.38
139 smb
445 smb
3306 MariaDB 5.5.5-10.3.15
```
Going to the SMB first since its a quick enumeration and with null account with a login into the SMB and we have `RAED/WRITE` on a share called `ITDEPT` and there is nothing in the share now going to the website

It says its `under construction` and there is a to do list so going to run a gobuster scan 
```
/logs
/cctv
```
Going to the `logs` folder and there is some logs there all empty but a `management.log` which leaks to users that are on the system `dawn,ganimedes` and dawn has a folder called `ITDEPT` so that might be the SMB going back to the logs we can see some cron jobs being ran but we also have access to the share were there ran at and there is nothing maybe we can put a reverse shell on the SMB and get a call back and we get a shell as `dawn` checking for `SUID` permissions we can run `zsh` to get root so going to gtfoBINS we can just go to `/usr/bin` and run `./zsh` and now we are root 