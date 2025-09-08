[https://ss64.com/nt/](https://ss64.com/nt/) - many windows CMD commands  
Environment variable for windows: %windir%
 
**C:\Windows\System32 - contains system critical files**  
Search for "other users" to see if you're administrator (will see option to add a user)
 
**C:\Users** - where **users** are stored
 
**C:\Windows\System32\Config\SAM** (or SYSTEM) - where **passwords** are stored (sometimes c:\Windows\Repair\) (SYSTEM contains the encryption key for the hashes in SAM).
 
Search "**MSConsole**" in the search bar for the configuration app. Can find many tools from [there](https://docs.microsoft.com/en-us/troubleshoot/windows-client/performance/system-configuration-utility-troubleshoot-configuration-errors).  
-Right click on start button; type "lusrmgr.msc" and okay to access user info.
 
C:\Windows: This is where environment variables, more specifically system environment variables, come into play. Even though not discussed yet, the system environment variable for the Windows directory is %windir%.
 
**Task manager** has info about system processes (ps -ef on linux). Right click taskbar>Task Manager>More Details or (Ctrl+Shift+Esc) (taskmgr)  
Task Manager>Performace = IP Address and CPU/GPU models
 
Another method to view **environment variables** is Control Panel > System and Security > System > Advanced system settings > Environment Variables **OR** Settings > System > About > system info > Advanced system settings > Environment Variables
 
**Kerberos uses hostnames**  
**NTLM uses IP address**
 
**NTFS**:  
The file system used in modern versions of Windows is the New Technology File System or simply NTFS.  
NTFS is known as a journaling file system. In case of a failure, the file system can automatically repair the folders/files on disk using information stored in a log file. This function is not possible with FAT.

![Permission Read Write Read & Execute List Folder Contents Modi%' Full Control Meaning for Folders Permits viewing and listing of files and subfolders Permits adding of files and subfolders Permits viewing and listing of files and subfolders as well as executing of files; inherited by files and folders Permits viewing and listing of files and subfolders as well as executing of files; inherited by folders only Permits reading and writing of files and subfolders; allows deletion of the folder Permits reading, writing, changing, and deleting of files and subfolders Meani'" for Files Permits viewing or accessing of the file's contents Permits writing to a file Permits viewing and accessing of the file's contents as well as executing of the file N/A Permits reading and writing of the file; allows deletion of the file Permits reading, writing, changing and deleting of the file ](Exported%20image%2020250424155710-0.png)

To find permissions: properties>security>group/user names
 
Alternate Data Streams (ADS) is a file attribute specific to Windows NTFS (New Technology File System).  
Every file has at least one data stream ($DATA), and ADS allows files to contain more than one stream of data. Natively Window Explorer doesn't display ADS to the user. There are 3rd party executables that can be used to view this data, but Powershell gives you the ability to view ADS for files.
 
## MSConsole Tools:

Computer management (compmgmt) > Task Scheduler (like CronTabs)  
>Event Viewer (log of what has happened on comp)  
Errors:

![The following table describes the five event types used in event logging. Event type Error Warning Information Success Audit Failure Audit Description An event that indicates a significant problem such as loss of data or loss of functionality. For example if a service fails to load during startup, an Error event is logged. An event that is not necessarily significant but may indicate a possible future problem. For example, when disk space is low, a Waming event is logged. If an application can recover from an event without loss of functionality or data, it can generally classify the event as a Waming event. An event that describes the successful operation of an application, driver, or service. For example, when a network driver loads successfully, it may be appropriate to log an Information event. Note that it is generally inappropriate for a desktop application to log an event each time it starts. An event that records an audited security access attempt that is successful. For example, a user's successful attempt to log on to the system is logged as a Success Audit event. An event that records an audited security access attempt that fails. For example, if a user tries to access a network drive and fails, the attempt is logged as a Failure Audit event. ](Exported%20image%2020250424155711-1.png)

Windows Events:

![The event log contains the following standard logs as well as custom logs: Application Security System CustomLog Description Contains events logged by applications. For example a database application might record a file error. The application developer decides which events to record. Contains events such as valid and invalid logon attempts, as well as events related to resource use such as creating, opening, or deleting files or other objects. An administrator can start auditing to record events in the security log. Contains events logged by system components, such as the failure of a driver or other system component to load during startup. Contains events logged by applications that create a custom log. Using a custom log enables an application to control the size of the log or attach ACLs for security purposes without affecting other applications. ](Exported%20image%2020250424155712-2.png)

>shared folders (to see what each user is running, files opened, accesses)  
>Performance>Perfmon (to see perfomrance data for trouble-shooting)  
>Storage>Disk Management (add/change disks, partitions)  
>System information (msinfo32) info _hardware, system components, and software environment_  
>Resource Monitor (resmon) CPU, Disk, Network, Memory info  
>command prompt (cmd)  
>Windows Registry (Registry Editor (regedit/regedt32.exe) to view/edit registry; holds user info, apps, ports)
 
/// sometimes windows running FileZilla FTP stores passwords in:  
C:\Program Files\FileZilla Server\FileZilla Server.xml  
or  
C:\xampp\FileZilla Server\FileZilla Server.xml
 
## to add user to windows admin group:

net user <username> <password> /add  
net localgroup administrators <username> /add
 
## <><> login to windows over RDP <><>

xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:MACHINE_IP /u:USERNAME /p:'PASSWORD'