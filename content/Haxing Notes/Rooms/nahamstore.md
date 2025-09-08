[https://tryhackme.com/room/nahamstore](https://tryhackme.com/room/nahamstore)
 
ffuf -u [http://nahamstore.thm](http://nahamstore.thm) -w /directory-list.txt -H "Host:FUZZ.nahamstore.thm" -fw 125  
dirbuster on each subdomain  
sublist3r not working for some reason
 
Fuff:  
10.10.194.10 [www.nahamstore.thm](http://www.nahamstore.thm)  
10.10.194.10 shop.nahamstore.thm  
10.10.194.10 marketing.nahamstore.thm  
10.10.194.10 stock.nahamstore.thm
 
/:  
/search (Status: 200)  
/login (Status: 200)  
/register (Status: 200)  
/uploads (Status: 301)  
/staff (Status: 200)  
/css (Status: 301)  
/js (Status: 301)  
/logout (Status: 302)  
/basket (Status: 200)  
/returns (Status: 200)
 
shop:  
# redirect to /
 
marketing:  
none
 
stock:  
/product (nothing imp)
 
nahamstore-2020-dev:  
/api
 
/api/customers/
 
"customer_id required"
 
/api/customer?customer_id=2