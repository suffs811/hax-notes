### Overview of functionality
##### Users
- register
- login
- verify session id
##### get data
- ?q= - search for recipes
- specific recipe id
- all puns
- specific pasta pun id
- admin - get all recipes
- (internal only) internal actions
##### Hidden directories
- /v1/data/internal/users - 401
----
## Initial Ideas
- [ ] Find api endpoints
- [ ] Authentication
	- [ ] fuzz for admin token
- [ ] BOLA/BFLA
	- [ ] Try other methods for each endpoint
- [ ] Injection
	- [ ] NoSQLi works on recipes?q=
- [ ] Mass Assignment
- [ ] Excessive Data Exposure
	- [ ] usernames (chef names) from /recipes
- [ ] SSRF
	- [ ] 
- [ ] Chaining Attacks

# Findings
### Chained Attack (SSRF/BFLA/Data Exposure)
Adding the internal endpoint /v1/data/internal/users as the "original recipe" in an uploaded recipe, then GETing the recipe pulls back all users (SSRF/BFLA) including admin and users/password/sessionIds (excessive data exposure). 
### Mass Assignment
The users endpoint shows the userlevel:admin for the admin user. When creating a new user, you can specify this variable and it gets added to the new user, giving them admin status.
### NoSQL Injection
Multiple endpoints are vuln to NoSQLi including /v1/auth/check, /recipes?q=. and /admin/recipes, allowing you can verify any known username's session, or get the first username (admin in this case) by using the payload for both variables in /auth/check, and bypass admin auth to get all recipes.
### Authentication
Can create recipes on behalf of other users by inserting their sessionId taken from chained attack to the recipe, or using NoSQLi.
Guessable sessionIds. Could possibly guess another user's token.
