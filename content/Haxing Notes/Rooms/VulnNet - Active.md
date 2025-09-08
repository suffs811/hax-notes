> https://tryhackme.com/room/vulnnetactive

#thm/windows
#thm/medium
****
# Enumeration
### Nmap
| PORT      | STATE | SERVICE         | REASON  | VERSION                                     |
| --------- | ----- | --------------- | ------- | ------------------------------------------- |
| 53/tcp    | open  | domain          | syn-ack | ttl 125 Simple DNS Plus                     |
| 135/tcp   | open  | msrpc           | syn-ack | ttl 125 Microsoft Windows RPC               |
| 139/tcp   | open  | ==netbios-ssn== | syn-ack | ttl 125 Microsoft Windows netbios-ssn       |
| 445/tcp   | open  | microsoft-ds?   | syn-ack | ttl 125                                     |
| 464/tcp   | open  | ==kpasswd5==?   | syn-ack | ttl 125                                     |
| 6379/tcp  | open  | ==redis==       | syn-ack | ttl 125 Redis key-value store 2.8.2402      |
| 9389/tcp  | open  | mc-nmf          | syn-ack | ttl 125 .NET Message Framing                |
| 49665/tcp | open  | msrpc           | syn-ack | ttl 125 Microsoft Windows RPC               |
| 49667/tcp | open  | msrpc           | syn-ack | ttl 125 Microsoft Windows RPC               |
| 49668/tcp | open  | msrpc           | syn-ack | ttl 125 Microsoft Windows RPC               |
| 49670/tcp | open  | ==ncacn_http==  | syn-ack | ttl 125 Microsoft Windows RPC over HTTP 1.0 |
| 49672/tcp | open  | msrpc           | syn-ack | ttl 125 Microsoft Windows RPC               |
| 49680/tcp | open  | msrpc           | syn-ack | ttl 125 Microsoft Windows RPC               |
| 49732/tcp | open  | msrpc           | syn-ack | ttl 125 Microsoft Windows RPC               |
| 6379/tcp  | open  | ==redis==       | syn-ack | ttl 125 Redis key-value store 2.8.2402      |
- DNS
- SMB
- 464: The fact you're seeing this service and port suggests you may be scanning a Domain Controller, for which both UDP & TCP ports 464 are used by the Kerberos Password Change. This port in particular is used for changing/setting passwords against Active Directory.
- .net message framing (soap message framer)
- redis
