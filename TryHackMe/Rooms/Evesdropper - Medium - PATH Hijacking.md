The room gives us a `id_rsa` key for the user frank and we have to get root I downloaded linpeas on to the host and did not find anything so ima put pspy64 and see if there is any cron jobs running there is one interesting thing running a `sudo cat /etc/shadow` time to check if we can edit are paths and we can edit the `.bashrc` file so now we need to make a script to capture the shadow file 
```
#!/bin/bash
read shadow_pass
echo $shadow_pass >> /tmp/passwd
```
and then chmod +x the file and then edit are `.bashrc` file and now we have to re log in and then do `echo $PATH` and are path updated and now we wait for the cron to make are password file and then we need to edit the path agin and re log in there is also a page on hacktricks about this now we have the password for frank `!@#franki.................` and need to edit the PATH agin and re log and when we do `sudo -l` we can do anything as root so I did sudo -u root bash and now we are root 