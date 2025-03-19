Started with a nmap scan
```
22 OpenSSH 8.9p1
80 Apache 2.4.59
```
When going to the site there is a login page and a register page looking at the login page when we try basic sqli query's and it says there is anti bruteforce in place when going to the register page we can make a account and login and we see the log of the last logins but we don't see much else there is no links or anything so going to run a gobuster scan
```
/css
```
Did not much anything else during this I was testing xss and did not get anything I tried sqli injection with `/" UNION SELECT 1 -- -` and when we login after we make the account we get a sql error now we just need to find the names of the tables and how many there are 2 tables and if we look at the `users` tables we get 3 columns `id,username,pass` now we need to dump the passwords and the users and we can do so with this payload
`/" union select 1,SUBSTRING((select group_concact(password) FROM users WHERE username='admin'), 1, 16) -- -` and this gives a partials hash `/" union select 1,SUBSTRING((select group_concact(password) FROM users WHERE username='admin'), 17, 16) -- -` now we can do the same for the other users `foo` and `bar` since the admin hash did not seem to crack  we need to the hash turns out its the passowrd for the admin and we can login into the server 
