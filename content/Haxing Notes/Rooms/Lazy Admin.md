[https://tryhackme.com/room/lazyadmin](https://tryhackme.com/room/lazyadmin)
 
10.10.152.247
 
22 ssh  
80 http
 
/content (gobuster)
 
javascipt file tells us that sweet rice version 0.5.4  
find cve in searchsploit
 
find mysql backups directory and look for admin and password in the mysql php file
 
manager  
Password123
 
change <php> section in exploit to a php reverse shell, change ip/port  
firefox exploit.html  
in ads section of admin page see if present  
nc listener  
go to ip/content/ads/revshell.php (or whatever 'value' is in the php code) (found in exploit notes)
 
sudo -l  
cat backup.pl, changes file called /etc/copy.sh  
paste a netcat revshell in copy.sh  
change ip and port in revshell  
nc listener  
then sudo /usr/bin/perl /path-to/backup.pl  
should have root
   

/usr/bin/script -qc /bin/bash /dev/null (to get a stable shell if you don't have python)