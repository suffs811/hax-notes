#evasion [nmap WAF identification](https://nmap.org/nsedoc/scripts/http-waf-detect.html) [ASE WAF and Rate Limit Bypass](https://appsecexplained.gitbook.io/appsecexplained/bypassing-controls/waf-bypasses)
# Fingerprinting
***
Use `wafw00f` to identify if a WAF is present and what kind it is
`nmap -p 80 --script=http-waf-detect <host>`
# Evasion/Bypass
***
> [!tip]
> 1. Even if a WAF is present, try to fuzz the input with a list of SQLi payloads (using Burp or ffuf) and try to see if there are any syntaxes that are able to bypass the WAF i.e. the WAF might not be perfectly tuned to block ALL SQLi payloads.
> 2. Craft your payload from there.

1. **Encoding Evasion**: Use URL, Unicode, Base64, or other encodings to disguise payloads.

2. **HTTP Parameter Pollution**: Manipulate parameters to exploit the way the WAF processes multi-instance parameters. (One of my favourite techniques!)

3. **Session Splicing**: Divide the attack into multiple requests or sessions to disrupt the WAF's ability to correlate the events.

4. **Verb Tampering**: Change the HTTP method (GET, POST, HEAD, etc.) to an unconventional one that the WAF might not inspect.

5. **Path Obfuscation**: Include irrelevant path information that gets ignored by the server but confuses the WAF (like using directory traversal techniques).

6. **Query String Manipulation**: Alter the query string with special characters or payloads that might be overlooked by the WAF.

7. **Header Manipulation**: Modify HTTP headers such as `User-Agent`, `Referer`, or custom headers in ways that are not expected.

8. **Cookie Poisoning**: Inject payloads into cookie values which may not be inspected or properly sanitized by the WAF.

9. **Content-Type Evasion**: Use unusual or mismatched content-types in the HTTP header to bypass checks that are content-type specific.

10. **Extension Manipulation**: Changing file extensions or using obscure ones to evade filters that inspect file names.

11. **Protocol-Level Evasion**: Utilize discrepancies in protocol implementations (like ambiguous requests) that may be differently interpreted by the WAF and the target web server.

12. **Attack Obfuscation with Legitimate Requests**: Mix in legitimate traffic with the attack traffic to reduce the anomaly score that might otherwise trigger the WAF.

13. **Bypassing with JavaScript**: Use JavaScript to construct the final payload in the client-side browser, which may not be executed or recognized by the WAF.

14. **Using Comment Injection**: Place comments within SQL statements or scripts to disrupt signature detection.

15. **Utilizing Server-Side Request Forgery (SSRF)**: Exploit the server's functionality to make requests that bypass the WAF's rules.

16. **Timing Attacks**: Execute actions with delays, leveraging the fact that some WAFs have a time window for rule execution.

17. **Ruleset Flaws**: Exploit known weaknesses in the rulesets employed by popular WAFs, which are sometimes documented by security researchers.
# Rate limiting Bypass

### What is it?

Rate limiting prevents us from sending large numbers of requests to a target. It can also be referred to as throttling.

A simple example:

- An application has a login form
    
- When a request is made to login, the IP is saved and a counter assigned
    
- If more than 10 attempts are made within 1minute the IP is blocked
### Checklist

- Can we identify how the rate-limiting is being applied?
    
- Can we spoof the a header that's being used
    - `X-Real-IP`
        
    - `X-Forwarded-For`
        
    - `X-Originating-IP`
        
    - `Client-IP`
        
    - `True-Client-IP`
- Can we use other user agents?
    
- Can we use different cookies or session tokens?
    
- Can we tamper with HTTP verbs
    
- Can we decrease the frequency of requests and leave overnight?
    
- Can we create legitimate-looking behaviour
# Tools
***
wafw00f