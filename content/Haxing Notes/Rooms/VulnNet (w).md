### nmap -vv -sS -Pn -oN 10.10.26.66.nmap -p- 10.10.26.66

53/tcp open domain syn-ack ttl 125  
135/tcp open msrpc syn-ack ttl 125  
139/tcp open netbios-ssn syn-ack ttl 125  
445/tcp open microsoft-ds syn-ack ttl 125  
464/tcp open kpasswd5 syn-ack ttl 125  
6379/tcp open redis syn-ack ttl 125  
9389/tcp open adws syn-ack ttl 125  
49666/tcp open unknown syn-ack ttl 125  
49668/tcp open unknown syn-ack ttl 125  
49669/tcp open unknown syn-ack ttl 125  
49670/tcp open unknown syn-ack ttl 125  
49673/tcp open unknown syn-ack ttl 125  
49693/tcp open unknown syn-ack ttl 125  
49778/tcp open unknown syn-ack ttl 125
 
### nmap -vv -sC -sV --script=vuln -p 53,135,139,445,6379,49666,464,9389 10.10.26.66

PORT STATE SERVICE REASON VERSION  
53/tcp open domain syn-ack ttl 125 Simple DNS Plus  
135/tcp open msrpc syn-ack ttl 125 Microsoft Windows RPC  
139/tcp open netbios-ssn syn-ack ttl 125 Microsoft Windows netbios-ssn  
445/tcp open microsoft-ds? syn-ack ttl 125  
464/tcp open kpasswd5? syn-ack ttl 125  
6379/tcp open redis syn-ack ttl 125 Redis key-value store 2.8.2402  
| vulners:  
| cpe:/a:redislabs:redis:2.8.2402:  
|_ CVE-2021-32626 6.5 https://vulners.com/cve/CVE-2021-32626  
9389/tcp open mc-nmf syn-ack ttl 125 .NET Message Framing  
49666/tcp open msrpc syn-ack ttl 125 Microsoft Windows RPC  
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
 
Search Redis exploit
 
Try:  
Redis-cli -h IP  
>INFO  
>CONFIG GET *  
Scroll down, find User: "C:\\Users\\enterprise-security\\Downloads\\Redis-x64-2.8.2402"  
Use EVAL command to see if you have RCE