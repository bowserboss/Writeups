Started with a nmap scan
```
80 webserver?
4346 webserver?
```
Going to the webserver on port `80` cant find much going on with the site so looking at the source code there is a `/api` in the javascript file and we see another path called `/new-booking` this sets a cookie for us that's in `base58` and there is a new js file we can find called `new-booking.js` that makes a call to `/api/booking-info.php=` and takes are booking key 
Time to look at the other webhost and run a gobuster on it as well 
```
/mail
/ws
```
these endpoints lead us to nothing looking back at the boking info we could test to the booking key for sql injection when we do `1'` we get bad request to further test this we when run this payload `1' order by 3 -- -` when get bad request so we do have sql injection and only have 2 columns so now we can try doing a select statement `1' UNION SELECT 1,2 -- -` and we get a room number and amount of days now time to try and get the version of the database with this payload we can `1' UNION SELECT 1,sqlite_version() -- -` and we get `3.42.0` now we can go to payload of all things and get some sql commands we can run and we get the tables names 
```
email_access
reservations
bookings_temp
```
now we can get the columns for the tables and then dump them and we get a login for the web service `pdenton:...............` looking at the javascript for the site we are make a socket connection to the `ws` and testing around this this at the bottom of the script there is a format we can follow to talk to the socket and we can test this with `socket.send("$(ls)")` and we get very minimal output but we do see `bin` in the top left corner now so we can make a bash script and try getting a reverse shell with it and we did now we are `gilbert` going to the home folder we a note with `gilbert` password in it `ilov...................r` and running `sudo -l` we can run `/usr/sbin/ufw status` as root running linpeas we found there is a folder called `/srv` and has a note in it called `.dad` and taking a guess there is Sandra's password in it `anywhe.............` and now we are `sandra` looking in the home folder we find a Pictures folder so using netcat we transferred the file back to my machine and if you open the picture it has jojo password in it `kingofhe.................` so now we are `jojo` time to get root we setup a nfs server on are own host and then mount to it from the box and transfer bash over and we can get root 