### Download postman and install as usually.

in this the task we did earlier was with different interations of content webpages but 
in postman endpoints will be given by developers.

In postman open import and select the collection.

after that dropdown the collection and select login and set url if not presnt 
" (http://crapi.apisec.ai/login)"
in body tab set the email and password correctly of before
{"email": " test999@test.com", "password":"Aa@12345"}


you will get a token after clicking send.

than right click the collection and select edit and tab to authentication

type= bearer token

 and paste the token we got so we dont need to sign every time and see everting on the webiste 

now to test in burp suite we go to postman -> stteings -> proxy

turn off "use system proxy"
turn on "use cutom proxy config"

proxy type on HTTP.,HTTPS

and give burp proxy in the proxy server


now if i click get dashboard in postman  and send reuest i will get the request
in burp suite and work with it like the older tasks similarlly.

### if in url i can change id and get value of other users than that vulnerability is called BOLA 

IDOR is simply another type of Broken Access Control issue. IDORs
happen when users can access resources that do not belong to them
by directly referencing the object. Object can be ID, number or
filename.

### if i am a user and want to delete some of my data but i dont have access as i am a user but still can gain admin access and delete it than that is called BFLA (broken function level authorization)

### process of attack

at first see the video id in burp request

also if we want to see if we can do BOLA than we can go to intruder tab and select the id and click "Add $" than on filter payload type Numbers from 1 to 100 

 

 



