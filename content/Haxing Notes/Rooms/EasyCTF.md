ip: 10.10.131.42  
Ports:  
21 ftp  
80 http  
2222 ssh
 
User: mitch  
Pass: secret
 
Nmap scan
 
Use dirbuster to find hidden web directories / robots.txt
 
/simple - directory found
 
Server running CMS 2.2.8 / search for vulnerability searchsploit
 
Use CVE (trouble with packages)
 
hashcat -O -a 0 -m 20 <password salt>:<password> <path to rockyou.txt>
 
Ssh using username and password
 
*User flag*
 
Sudo -l (to find what user can run as root)
 
Search gtfobins for vim exploits
 
Use :!sh in a vim file
 
Root
 
Cd /root
 
*root flag*