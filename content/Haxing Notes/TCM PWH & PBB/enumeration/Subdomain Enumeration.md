### Tools
- https://crt.sh
- `subfinder -d <domain>`
	- + `httprobe`
- `assetfinder | sort -u`
- `amass enum -d <domain>`
- `gowitness`
- nuclei
- katana
- censys search
	- https://search.censys.io/search?resource=hosts&sort=RELEVANCE&per_page=100&virtual_hosts=EXCLUDE&q=$domain
### Gowitness
```
mkdir pics
gowitness -f domains.txt -P pics --no-http
```
Make sure domains list does not have https://
### Script
subdomain-annihilator
