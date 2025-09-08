#checklist/web [AppSecExplained Guide](https://appsecexplained.gitbook.io/appsecexplained/discovery-recon/methodology)
***
### Workflow
1. Turn on Burp (enable setting to prevent caching so you always get data back from reqs)
2. Try out every page and functionality on the site
3. Write down the main functions of the app (register/login, store/checkout, etc.)
4. Think of which vulns are possible for each category
5. Make a list of exploits to try against each vuln

> [!abstract] Enumerate
> Find:
> - [ ] Subdomains
> - [ ] Directories
> - [ ] Fingerprint
> 	- [ ] Censys
> 	- [ ] Builtwith
> 	- [ ] securityheaders
> - [ ] Automated scans
> 	- [ ] ZAP
> 	- [ ] Burp Pro
> - [ ] Login portals
> - [ ] Open ports
> - [ ] Look for WAF
> - [ ] Stack Set (wappalyzer / whatweb)
> - [ ] Web engine / templating engine / Wordpress versions
> - [ ] Vulnerable third-party components
> - [ ] Minor exploits you can chain together
> - [ ] What can unverified user email do compared to a verified user?

> [!warning] User Input
>If you find a place to submit user input (login page, comment form, contact form, search bar, edit panel, etc.) or URL parameters or JSON body data, try:
> - [ ] SQLi
> - [ ] XSS (Test on all potential pages/inst)
> - [ ] XXE
> - [ ] IDOR
> - [ ] Path Traversal / LFI / RFI
> - [ ] Template Injection / SSTI / CSTI
> - [ ] Command Injection
> - [ ] API endpoints with broken access controls/using valid token with other username/trying PUT and DELETE
> - [ ] SSRF
> - [ ] CSRF
> - [ ] Vulnerable services
> - [ ] HTTP Request Smuggling

> [!danger] Next Attacks
> - [ ] Insecure File Upload
> - [ ] Open redirects
> - [ ] Subdomain takeover
> - [ ] Mass Assignment
> - [ ] WebSocket Hijacking
> - [ ] Authentication Attacks
> 	- [ ] JWT
> 	- [ ] Brute Force credentials
> 	- [ ] Guessable session tokens
