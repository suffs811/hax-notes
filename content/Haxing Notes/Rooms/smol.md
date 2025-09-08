22  
80
 
use wpscan, find vulnerable plugin, search for exploit (LFI/SSRF)
 
look at wp-config.php for user database credentials, login to wp-admin
 
see private note about 'hello dolly' source code, find the plugin info page online (wp-content/plugins/hello.php)
 
decode the base64, see that you need to add ?cmd= parameter to request at dashboard
 
wget a php rev shell and execute it (php shell.php)
 
look at opt/ for mysql backup, look at wp_users, find admin creds
 
crack creds with john the ripper.  
cracked passwords for gege and diego
 
su diego from webshell with the password  
look around home dirs, find id_rsa for think, ssh using think's private key (chmod 600 id_rsa&&ssh -i id_rsa think@ip)
 
su gege  
download the wordpress zip file from home dir  
use ziptojohn to find archive password (its gege's password from earlier sql backup)  
unzip the archive and look at wp-config.php for xavi's creds
 
su xavi, sudo -l, sudo su
 
/root/root.txt
   

# Lessons Learned

Exploiting WP plugins (jsmol2wp for LFI, hello dolly source code for RCE (webshell))  
Dumping creds from sql backup  
cracking passwords with john  
find credentials in wordpress archive folder  
pivot between users on a host with different permissions  
get root