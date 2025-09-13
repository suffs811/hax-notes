When you assign a value to a parameter that was not intended to be assigned by the developer

> [!tip] 
> i.e. suppose when you create a new user it asks for `username`, `password`, and `country`, but you also include `isAdmin=true` parameter, overwriting the backend assignment of `isAdmin=false` to the new user
#### Ways to find available parameters:
- Open source code
- JWT claims sections ({"isAdmin":"false"})
- The returned data from an endpoint (i.e. /account or /user etc.)
#### Another example
If you are able to upload a new product to a product page (via POST req for example), and assign values to the variables, i.e.
	{"name":"car"}
	{"price":"0.00"}
this is mass assignment. Also, bonus points if you set the price to `-100.00` and see if it credits $100 to your account when you buy it.
