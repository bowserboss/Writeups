Started with the file it wants us to download and we need to extract something out of it when we run exiftool it has a cyber chief link in it and when we do `steghide` it has a password so I used `stegseek` and cracked the pass to `pepper` so now we can extract what's inside the image after we extracted it we get a file called `lone` it looks base64 encoded to we decode it with `base64 -d lone` we get a gzip file so I changed it to `lone.tgz` and used tar to extract it and its a `id_rsa` key file  now we need to find a user for this rsa key time to start the second part of the challenge

Started with a nmap scan
```
22 OpenSSH 7.6p1
222 OpenSSH 8.4
```
We can use this key we got to auth to the ssh on port `222` with the user lone was just a guess and it was right we then get to a screen saying this is pyLon password manager and using the password we got eailer `pepper` and using the cyber chief recipe we found in the image we get the encoded key we need to enter and we get to a new page with 4 options decrypt a password , create new password, delete a password, search password going to the first option we get the first flag and there some other stuff in there also getting the password for the `python.thm` site its the password for the user lone for SSH `+2BRkR.............` and now were on the host after looking a around a bit I find a `.git` folder and using my git commands I switched to the first repo and then run the `pyLon_pwMna.py` script and using the same password as before we can open it and it has the creds for the gpg file in are home folder and we can decode the file and we get the password for the user `pood` `........` now as user `pood` we can edit the openvpn file and then run it with the user `lone` to get root we edit the ovpn file and add the up command with a bash rev shell and we get a call back as root now to access the final flag we need to edit the root password in the shadow file to what ever password we want and then we can decrypt the gpg file and get the flag 