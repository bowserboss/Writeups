Started with a nmap scan
```
22 OpenSSH 8.2p1
1337 APache 2.4.41
```
Going to the website there is a note in the source code that says all directory's need to start with `hmr_` so we can use ffuf and take the request and do some directory busting 
```
/imgaes
/css
/js
/logs
```
Going to the logs folder there is a error.log file and it has a email `tester@hammer.thm` and then going to the forgot password forum on the site once we enter the email we got it ask us for a 4 digit number but when trying to brute force it we get a rate limit get hits and we have to clear are cookies but to bypass this we can use the `X-Forwarded-For:` to bypass this so we make a list of numbers and a list of fake ips and then we can use ffuf to brute force the login 
`ffuf -w 9999.txt:W1 -w fake_ips.txt:W2 -u "http://10.10.187.210:1337/reset_password.php" -X POST -d "recovery_code=W1&s=80" -b "PHPSESSID=cookie" -H "X-Forwarded-For:W2" -H "Content-Type: application/x-www-form-urlencoded" -fr "Invalid" -mode pitchfork -fw 1 -rate 100 -o output.txt` 
Now we got the code we enter it on the site and we can change the password and we can login in now and we got the first flag now doing some enumeration we can run the ls command and find a key file `188ade1.key` and since its a jwt token we can try and modify it and using the key we found we can sign a admin key and send the request to burp and we can get the flag now