Started with a nmap scan 
```
22 OpenSSH 7.6p1
139 smb
445 smb
```
Starting with the smb since that's all we have there is 2 shares 
```
Anonymous
IPC$
```
In the anonymous share there is a `journal.txt` file its a txt file with a bunch of base64 that converts to a PNG file also I got enum4linux running on the host in the background and got some users 
```
johan
lily
```
Going back to the PNG file using binwalk shows that there is something compressed in the image so ima use a program called `stegpy` to extract the info I could not install it in the pip version so I had to use the github folder and install it that way and now we have the extracted data of the `_journal.zip` but when you run file on it its a JPEG file we need to switch it back to a zip file the magic numbers for this is `50 4B 03 04` and were back to a PKZIP file and using zip2john we can get it to a hash we can crack and it cracked to `septe......` so now we can extract the zip file and we get a `journal.ctz` when running file no this its a `7z` file running `7z x journal.ctz` it needs a passowrd so now we need to turn this file into a hash so we can crack it using a program called `7z2hashcat` and now we have a big hash we can crack with hashcat and it cracked to `tige......` now we get a file called `Journal.ctd` running file on it its a XML document it is a journal about the person and it has a encoded password list in it 4 of them we going to try the first list on the 2 users I found and there some other users in the txt file using netexec to try and crack the smb first did not get anything so trying the SSH service cracked the SSH password for the user `lily:...........` looking around the file system in the `/var/backups` the `shadow.bak` file we can read and get all the users hashes and try and crack them we cracked the user `johan:...........` and we got the user flag now and going for root there is lots of exploits we can run I ran PwnKit but its also vulnerable to a perl exploit