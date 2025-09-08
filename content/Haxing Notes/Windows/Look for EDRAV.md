PS C:\> wmic /namespace:\\root\securitycenter2 path antivirusproduct  
PS C:\> Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntivirusProduct  
PS C:\> Get-Service WinDefend  
PS C:\> Get-NetFirewalLProfile  
PS C:\> Get-EventLog -List  
PS C:\> wmic product get name,version