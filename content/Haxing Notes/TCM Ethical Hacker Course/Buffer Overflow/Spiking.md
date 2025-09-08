Find executables/commands, throw a bunch of characters at it, if it crashes it is vulnerable
 
## Tool: Generic_send_tcp

### ./generic_send_tcp <ip> <port> file.spk 0 0
 
File.spk:  
s_readline();  
s_string("<Name_of_command_to_overflow> ");  
s_string_variable("0")'
 
Watch Immunity Debugger for errors/crash/violation
 
Look for EBP and EIP, if EIP is all A's (41414141), it is vulnerable
 
Vulnerable command is found.