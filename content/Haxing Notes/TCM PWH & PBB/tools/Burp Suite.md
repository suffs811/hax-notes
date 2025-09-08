# Helpful Tips
>Right-click on a request in the left hand side or go to Target>scope settings to set the scope only for the target domain

>Go to the bApp Store and install extensions like Logger++ and more to extend Burp's capabilities
# Sniper Attack
***
> [!todo] Todo
> 1. Send the request to Intruder
> 2. In the request, place the payload target symbol ($) on the password
> 3. Upload a wordlist to the payload
> 4. Run it
# Cluster Bomb Attack
***
> [!Todo]
> 1. Send the request to Intruder
> 2. Set attack type to cluster bomb
> 3. In the request, place the payload target symbol ($) on the username AND password
> 4. Create the payloads for usernames and passwords
> 5. Run
# Autorize (Burp Suite plugin)
***
1. Install Jython Standalone from Jython website
2. In Burp Extension settings, set the path of the Jython .jar file
3. Install Autorize plugin from bApp Store
4. On the right side, add a cookie with the JWT that will replace the one found in the sent request.
![[Pasted image 20250429220814.png]]
5. Proxy your cURL requests through burp
6. Look for 'bypassed' in the autorize plugin, which means the cookie was successfully replaced and it received a 200 code. 'Enforced' means it failed.
# LFI and Path Traversal
***
> [!todo] 
> 1. Send request to Intruder
> 2. Add the target symbols ($) to where the path traversal will take place
> 	1. litpro.cloud?file=\$recipe.txt\$
> 3. Set the payload to the built-in path traversal payload
> 4. Run it

# SQL Injection
***
> [!todo] 
> 1. Send request to Repeater
> 2. You can change the payload to test various SQLi injections
> 3. Remember to URL encode the payload as well
# Server-Side Template Injection
***
> [!todo] 
>1. Send request to Intruder
>2. Set target symbols around your target string
>3. You can use Burp's built-in SSTI wordlist as the payload
>4. If you get info about the templating engine, craft a payload for it and use it in Repeater
# Insecure File Upload
***
> [!todo] 
> Intercept the POST request and change the:
> 1. File extension
> 2. File MIME type / Magic Bytes (use hexedit or Burp)
> 3. File contents
>    
>Note: When changing the Magic Bytes at the beginning of the file, sometimes you either need to get rid of most of the file contents if they are getting in the way, or you need to only have a little bit before and after the web shell payload. Play around with it.
# Automatic Scanner (Commercial Version)
***
> [!todo]
> 1. Turn on the proxy in the browser
> 2. Manually crawl some of the pages on the site
> 3. Right click on the site in Burp Suite Target
> 4. Select Passively Scan This Host, it will crawl the site and find more pages
> 5. When finished, select Scan > Open Scan Launcher
> 	1. Select Audit Selected Items
> 	2. Consolidate Items
> 		1. Remove duplicates
> 	3. Manually remove pages with login/contact/comment/logout forms
> 6. Ok
# MFA Bypass
***
> [!Todo] 
> 1. If a login page requires an MFA code, proxy the request through Burp and use Burp/Fuff to fuzz the MFA code.
> 2. Also, once you have the MFA code, try to change the username in the proxied login request in case the server doesn't validate the username after the initial username/password login page 


