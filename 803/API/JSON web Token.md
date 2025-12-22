JWT is a token type which works as a cookie which stores a token which 
was created to first time login than you dont need to sign in again.

It has 3 parts

i. header (What type of algo is used)
ii. payload(username and time after when it gets expired)
iii. signature (encrypted msg used to validate token)


now open burp suite browser and search  http://crapi.apisec.ai/login

username= test999@test.com

password= Aa@12345


## Non Algo Attack

### process 
at first we will make header none so the algo is removed so if there is no algo 
than there is no need for signature so delete that whole.

now at first we need to go to  http://crapi.apisec.ai and create a vehicle and go to 
its request and right click it to open "Send to repeater"

We need to add a Extention "JSON Web Tokens"
AT first go to extensions tab than go to BApp Store than search for "JSON Web Tokens"

Now if there is a token than only the extension will be active in the repeater section.


in there make header none and use this to attack another user to get info by giving that mail id of the target in payload section and remove signature.

now if after click send get "Invalid token " it means that website doest have this vulnerabities and cant use this attack

in 103.117.194.181:3000 webpage we can use this attack
first use test as username to see.
do the same process if can access than need to make it admin to attack so change payload to admin.



 
