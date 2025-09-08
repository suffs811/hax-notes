# This assumes you have low level access to a host and are enumerating the host/network further
 
## runas.exe

Inject your credentials into memory for network auth
 _runas.exe /netonly /user:<domain>\<username> cmd.exe_  

Let's look at the parameters:
 
With the /netonly option, all network communication will use these injected credentials for authentication. This includes all network communications of applications executed from that command prompt window.
 
/netonly - Since we are not domain-joined, we want to load the credentials for network authentication but not authenticate against a domain controller. So commands executed locally on the computer will run in the context of your standard Windows account, but any network connections will occur using the account specified here.  
/user - Here, we provide the details of the domain and the username. It is always a safe bet to use the Fully Qualified Domain Name (FQDN) instead of just the NetBIOS name of the domain since this will help with resolution.  
cmd.exe - This is the program we want to execute once the credentials are injected. This can be changed to anything, but the safest bet is cmd.exe since you can then use that to launch whatever you want, with the credentials injected.
 
## SYSVOL

Sometimes creds can be in the C:\SYSVOL directory  
SYSVOL is a network folder on a domain controller is accessible by any authenticated AD account and stores GPO information
 
dir [\\<DOMAN>\SYSVOL\](file:///\\<DOMAN>\SYSVOL\)
   

## Microsoft Management Console

Use your injected creds from runas.exe to enumerate the host  
Enable RSAT on windows host if not already enabled:  
Press Start  
Search "Apps & Features" and press enter  
Click Manage Optional Features  
Click Add a feature  
Search for "RSAT"  
Select "RSAT: Active Directory Domain Services and Lightweight Directory Tools" and click Install
 
**Windows Start button, searching run, and typing in MMC**  
^ This will ensure the injected creds will be used in network auth
 
In MMC, we can now attach the AD RSAT Snap-In:
 
1. Click File -> Add/Remove Snap-in
2. Select and Add all three Active Directory Snap-ins
3. Click through any errors and warnings
4. Right-click on Active Directory Domains and Trusts and select Change Forest
5. Enter za.tryhackme.com as the Root domain and Click OK
6. Right-click on Active Directory Sites and Services and select Change Forest
7. Enter za.tryhackme.com as the Root domain and Click OK
8. Right-click on Active Directory Users and Computers and select Change Domain
9. Enter za.tryhackme.com as the Domain and Click OK
10. Right-click on Active Directory Users and Computers in the left-hand pane
11. Click on View -> Advanced Features
 
If everything up to this point worked correctly, your MMC should now be pointed to, and authenticated against, the target Domain:
 
## Command Prompt - Net

Net user /domain  
Net user <username> /domain  
Net group /domain  
Net group "Tier 1 Admin" /domain  
Net accounts / domain (to view password policy)
 
## Powershell

Complete list of [cmdlets](https://docs.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps)  
Utilizes injected creds from runas.exe
 
**enum users:**  
Get-ADUser -Identity <user.name> -Server za.tryhackme.com -Properties *
 
**enum AD groups:**  
Get-ADGroup -Identity Administrators -Server za.tryhackme.com -Properties *
 
**group members:**  
Get-ADGroupMember -Identity Administrators -Server za.tryhackme.com
 
**Get all AD objects:**  
Get-ADObject -includeDeletedObjects -Server za.tryhackme.com
 
**Get info about the domain:**  
Get-ADDomain -Server za.tryhackme.com
 
**Force change of password for user (must have AD-RSAT cmdlets enabled):**  
Set-ADAccountPassword -Identity <user.name> -Server za.tryhackme.com -OldPassword (ConvertTo-SecureString -AsPlaintext "<old_Password>" -force) -NewPassword (ConvertTo-SecureString -AsPlainText "<new_password>" -Force)
 
## BloodHound (SharpHound)

**On host machine:**  
SharpHound.exe --CollectionMethods All --Domain za.tryhackme.com --ExcludeDCs
 
**On kali:**  
neo4j console  
bloodhound --no-sandbox
 
scp <AD_Username>@THMJMP1.za.tryhackme.com:C:/Users/<AD_Username>/Documents/<Sharphound_ZIP_Path> .
 
## More techniques

[LDAP enumeration](https://book.hacktricks.xyz/pentesting/pentesting-ldap) - Any valid AD credential pair should be able to bind to a Domain Controller's LDAP interface. This will allow you to write LDAP search queries to enumerate information regarding the AD objects in the domain.  
[PowerView](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1) - PowerView is a recon script part of the PowerSploit project. Although this project is no longer receiving support, scripts such as PowerView can be incredibly useful to perform semi-manual enumeration of AD objects in a pinch.  
[Windows Management Instrumentation (WMI)](https://0xinfection.github.io/posts/wmi-ad-enum/) - WMI can be used to enumerate information from Windows hosts. It has a provider called "root\directory\ldap" that can be used to interact with AD. We can use this provider and WMI in PowerShell to perform AD enumeration.