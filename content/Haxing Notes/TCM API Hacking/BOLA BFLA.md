## BOLA (Broken Object Level Authorization)
> Basically when you can change the object-id in an api url and get info back on an object you shouldn't be authorized to see.

/api/v2/users/13255/address
> Changing 13255 to 13256 or a different user-id found on the website or through a different api call
## BFLA (Broken Function Level Authorization)
> Basically being able to do something (a function) that you shouldn't be able to do
> i.e. change other user's passwords, change a req to a DELETE req and delete a user, etc.

