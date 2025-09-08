## Types of Auth
#### Basic Auth
username:password encoded as base64
#### Bearer Token
A token is used for authentication, such as a UUID or random string
#### JWT / OAuth2.0 / API keys
A type of Bearer authentication. Most secure form of auth. Basically a signed token with extra info in it, such as privileges, username, exp date, etc.

---
## Types of Attack
#### Brute force
Lack of rate-limiting/WAF protection on API
#### Logic Issue
> Auth tokens using low randomness (i.e. you notice auth tokens are only a little big different each login)
1. Send auth request to Burp Sequencer
2. Configure custom location > highlight the token
3. Sequencer will try to find which characters differ from request to request
4. If token is base64 encoded, click 'decode base64 before analyzing' in the analysis settings
5. Create a list of potential tokens based on the pseudo randomness
	1. For example, if the token is just `user-000-domain.com` and sequencer shows that only the three numbers in the middle change on each login, try brute forcing the token `admin-000-domain.com` and every combination of numbers as the middle three numbers until you find a valid token for the admin user.
	2. Then use the successful admin token as a cookie, etc. to access the admin dashboard

> Weak JWT signatures
1. If a JWT is signed with a weak key AND is not of type RSA/RS but is HS/HMAC, you can use hashcat (-m 16500) or jwt_tool to brute force the key used in the JWT signature.
2. If found, you can use jwt.io to change the properties of the JWT (such as changing user to admin) and supply the signature key, which will provide you a valid JWT with the updated claims.
	`hashcat -a 0 -m 16500 jwt.txt /usr/share/wordlists/rockyou.txt`

#### Burp Suite has the JWT_Editor extension that allows you to test for common vulnerabilities such as alg:none, etc.
# JWT_Tool
> Get token details
> `jwt_tool <token>`

> Change/add data to the token
> `jwt_tool <token> -T`

> Specify a signing method for updating signature (if key is known)
> `jwt_tool <token> -S`

> Run all automated JWT tests on endpoint with mode [at]
> i.e. find vulnerabilities such as acceptance of broken/random/missing signature
> `jwt_tool -t https://domain.com/api/test -rf 'Authorization: Bearer <token>' -m at`
