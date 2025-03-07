Flags
1. check robots.txt file 3
2. takes the encoded string from the robots txt file and decode it multiple times and remove the spaces and its a web folder `DeskEL_secret_base` and then view source 
3. gobuster with `common.txt` you get a login forum view the source
4. sql injection in the login forum time based sqli and there is a database called `THM_f0und_m3` and a table called `nothing_inside` with the flag and there is a user table with a login ``
5. Login with the dump creds to get flag
6. Going to the main site look at response headers 
7. change cookie to 1
8. changing the user agent to a iphone 11 gives us this flag
9. found in source code of main site after clicking the tyre take it button 
10. going to the free sub page and make are referrer to tryhackme.com
11. going based off the hint if we post to the food page we can change the option to egg and we get the flag 
12. Main site fake jQuery js file and decode the string 
13. click on the red button 
14. Looking at the source there is a image commented out in the main page 
15. we can make a bash script to input letters that match the hint and we get the code `GameOver` 
16. go to game2 and in the request if we add button 2 and 3 and send it as one we get the flag
17. javascript on the main page we can decode it to `bin,dec,hex,ascii` 
18. make a GET request and in the request section add `Egg: Yes` and we get the flag
19. gobuster and get a image `/small` txt is in the image
20. the main page change request to post and set the content type and use the creds that it gives on the site and we get the last flag





