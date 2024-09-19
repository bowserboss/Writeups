Started with a nmap scan
```
22 OpenSSH 8.2p1
25 smtpd
110 Dovcot
143 Dovcot Ubuntu
993 Dovcot
995 Dovcot pop3d
4000 Node.js
50000 Apache 2.4.41
```
On the port `4000` its a webapp with friends did not look to much into it 
when going back and testing the activity field we can change are guest account to admin by in field one `isAdmin` field 2 `true` and now we have access to `seetings` and `api` when looking at the api we have 2 internal api and the sysMon app username `administrator` after taking one of the internal url's and we make it call on it self using the picture option in the settings and got the password `S$9$qk......` and this login is for the admin account on this app `adm....` so now we can move onto the sysMon app now we got creds
On port `50000` is the sysMon app says its for restricted app  both login apps use the same cookie after loging in if you look at the source page there is a `profile.php?img=` and we can get a LFI and look at `/etc/passwd` file with a lot of `....//` now we got a list of 2 ssh accounts on the host and I used hydra to see if we could crack the logins and I got the login for `josh...:......` 