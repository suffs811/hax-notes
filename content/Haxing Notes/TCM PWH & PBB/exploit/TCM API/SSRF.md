[[Haxing Notes/TCM PWH & PBB/exploit/SSRF|SSRF]]
##### When you can make a server make requests to itself or another private/internal endpoint to bypass CORS and authentication

### What to look for
> Any URL in the body/headers of an HTTP request (using Burp for example)

```
GET / HTTP/1.1
HOST: test.com
X-API-URL: /api/v1/data  <--*
Accept: */*

apiUrl: https://test.com/api/v1/data  <--*
```

1. Change the URL to the localhost (http://127.0.0.1) with a protected/internal endpoint (/admin)
2. Or change it to an external URL
3. The response should render/return the page