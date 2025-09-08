# Fuzzing
> [!tip] Tools
> Burp Suite
> WFuzz
> 
> Manual discovery

> Example API endpoint
> https://domain.com/api/v2/resources/authors?name=tanner

FUZZ for each part of the above URI, such as v2 (see if v0 or v1 exist), resources, authors, and the name parameter. You might find a local file inclusion vuln or an endpoint that's supposed to be secret.
#### Burp Suite
Surf the site and use all of its functionality while using Burp's proxy to capture any behind-the-scenes API calls
#### wfuzz
`wfuzz -c -z file,/usr/share/seclists/common.txt --sc 200 https://domain.com/api/v2/resources/authors?FUZZ=tanner`
# Source Code Discovery
In browser tools, go to Debugger, then look through js files