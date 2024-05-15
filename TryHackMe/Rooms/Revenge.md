Started with a nmap scan
```
22 OpenSSH 7.6p1
80 ngnix 1.14.0
```
Starting with a gobuster scan with list `2.3-medmium` and with `php and js` files also 
```
/index
/contact
/products
/login
/admin
/satic   403 forbidden
```
Started a nikto scan also it did not find anything


The login page does not seem to do anything its sending GET request and not POST
Test all the forms with sqlmap found a sqli in the product page with `#1*`

![[revenge1 1.jpg]]