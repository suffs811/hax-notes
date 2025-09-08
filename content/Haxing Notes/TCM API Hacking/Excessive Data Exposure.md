##### When an endpoint returns more data (especially sensitive data) than is necessary for the UI

> [!tip] 
> This relates to a user's own data and other users' data

- /users/100
	- Same user's name, address, SSN, credit card number, birth date, etc. when only the username is needed
	- A different user's name, address, SSN, credit card number, birth date, etc.

> Usually paired with BOLA/BFLA/IDOR
