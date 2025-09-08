#### Tools
- Wappalyzer Browser Extension
	- Stack set
- https://builtwith.com/**
	- Stack set
- securityheaders.com
	- Request headers
- ssllabs.com
	- TLS info
- `curl -IL`
	- Headers
- nmap
	- Headers/services
- crt.sh
- socialhunter
- xssvibe
- sqlmap
- censys search
	- https://search.censys.io/search?resource=hosts&sort=RELEVANCE&per_page=100&virtual_hosts=EXCLUDE&q=$domain
- LinkFinder / SecretFinder
- XSSHunter (its a lot)
- HTTP-request-smuggler (Burp Suite App)
#### Find weak TLS ciphers:
`nmap -p 443 --script=ssl-enum-ciphers`