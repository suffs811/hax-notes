21  
22  
53  
1337 http  
1883 mosquitto
 
/admin  
/phpmyadmin - running mysql db
 
ffuf http with /dirb/big.txt
 
/admin_101 - need password
 
run sqlmap against /admin_101, dump passwords
 
find /file...., (from sql db) try with ?file=/etc/passwd  
find name of user, zeamkish  
enter name at /upload.... (from sql db), use burp to upload php rev shell  
read ssh creds in zeamkish home dir  
find suid files, use /usr/bin/find  
root