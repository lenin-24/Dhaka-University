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

## Cracking the Secret

here you can find common secret key used in signature

https://github.com/danielmiessler/SecLists/blob/master/Passwords/Leaked-Databases/rockyou-20.txt

Tool used = jwt tool

https://github.com/ticarpi/jwt_tool

download zip and install by 

create a wordlist.txt file in that folder than cmd in that folder 

and give command

pip install -r requirements.txt 

than run command

than go to 

https://43.230.120.10:8443/dashboard

username = test999@gmail.com
pass= Aa@12345

than go to click here to send mail in hog 
copy the pin and VIN

go to extentions and download json web tokens in BApp 
 
than on this link do right click and send to repeater
GET /community/api/v2/community/posts/re

python3 jwt_tool.py (paste the json token here ) -C -d wordlist.txt 

so we got the signature "carpi"

but the new website (https://43.230.120.10:8443/ ) doesnt let see signature
so we cant keep carpi in wordlist so it can match now if if we go to old ( http://crapi.apisec.ai)
and go to shop tab and copy that request and see the request token in https://www.jwt.io/ than we can see . 

now my mail is test999.com and want to access srk@gmail.com i need to crack its signature now i cracked my signature which is carpi

now i need to go to https://www.jwt.io/ 

and give my old token and edit the payload from test999.com to srk@gmail.com
and give signature carpi to generate an authenticate token to get target info.

paste the token in repeater and get the info 


## BFLA

go to http://crapi.apisec.ai/my-profile

change video name

take that PUT request to Repeater

change the PUT command to DELETE and give admin privileage in the url








 
