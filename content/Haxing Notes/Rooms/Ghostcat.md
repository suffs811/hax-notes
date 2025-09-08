nmap -vv -sC -sV -p- -T4 -oN
 
22  
53  
8009  
8080
 
see that 8009 is ajp apache2, has vuln, check out exploit on github, ajpshooter.py
 
skyfuck:8730281lkjlkjdqlksalks
 
scp
 
gpg2john tryhackme.asc > hash  
john --format=gpg --wordlist=rockyou.txt hash > key.txt  
alexandru
 
on target box:  
gpg --import tryhackme.asc  
gpg --decrypt credential.pgp
 
merlin:asuyusdoiuqoilkda312j31k2j123j1g23g12k3g12kj3gk12jg3k12j3kj123j
 
ssh  
sudo -l  
GTFObins zip  
/root/root.txt