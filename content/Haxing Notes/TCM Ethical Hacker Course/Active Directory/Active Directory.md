## Physical

Domain Controller (DC) is head honcho.  
AD DS Data Store holds the %SystemRoot%\NTDS\ntds.dit file (v important, pswd hashes), can only be accessed from the DC.
 
## Logical

Schema - defines types of objects  
Domain - groups objects together  
Tree - Hierarchy of domains (domain with subdomains)  
Forest - Hierarchy of trees  
Org Container (OU) - containers of objects (users, comps, groups)  
Object - User, group, computer, file shares, printers, etc.
 
Types of trust:  
Directional - trust both ways  
Transitive - only top level of hierarchies trust one another