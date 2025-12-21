JWT is a token type which works as a cookie which stores a token which 
was created to first time login than you dont need to sign in again.

It has 3 parts

i. header (What type of algo is used)
ii. payload(username and time after when it gets expired)
iii. signature (encrypted msg used to validate token)
