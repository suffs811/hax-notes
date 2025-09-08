Nmap  
80  
3389
 
/robots.txt - find poss password:  
UmbracoIsTheBest!
 
see it is using Umbraco
 
look around the website, find two flag in source code and two in metadata
 
google poem and find author's name; that's the admin's name  
Solomon Grundy
 
There was another email on one of the blog posts, we know admin email will be:  
SG@anthem.com
 
sg@anthem.com:UmbracoIsTheBest!  
using above creds ^, rdp into machine and see user.txt in desktop.
 
xfreerdp /u:'<username>' /p:'<password>' /v:'<domain (ex. controller.local)>'
 
go to file explorer, click View>show hidden items, see the "backup" directory with a file in it but we don’t have permission to open it. However we can right click>properties>security and add our user (sg) to the groups list and give the user complete authority. Now we can open the file.  
It has a potential password:  
ChangeMeBaby1MoreTime
 
Go to Administrator desktop, it prompts for password, enter above pass, read root.txt:  
THM{Y0U_4R3_1337}