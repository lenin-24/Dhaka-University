### Intro 

The are mainly two purpose of rate limiting Firstly, it helps API providers prevent abuse or overuse of their resources.
Without rate limits, a single user or application could make an excessive number of requests, potentially causing
performance issues for the API and affecting other users. Rate limiting acts as a safeguard against such scenarios.
Secondly, rate limiting is a tool for API monetization. It allows providers to offer different levels of service to different users
based on subscription plans. For instance, free users might have a lower rate limit, while premium users with a paid
subscription could enjoy higher limits, faster response times, or additional features.



At first on a website if i go to forget pasword in login section and give the mail id to sent otp than it sends a otp to the 
mail http://crapi.apisec.ai:8025/ to change password.
### now if i want to annoy that user 
i can capture the request send usng the burp suite and than right click the request and 
select "send to intruder" there select any number on the request and click "Add $"
than go to payload tab and 

payload set= 1
payload type = number

**payload settings**
from =1
to = 1000

than start attack

than 1000 otp will go to this mail http://crapi.apisec.ai:8025/


# API security broken function lvl Auth

### Here we will try to modify function even if we dont have access by changing request

like if i as a user want to change a video/image in dashboard and clickbutton to change name than in burp suite we select that request and right click "Send to repeater" 

than click send to generate the response

now with PUT method we can change/update a function than we can use DELETE to delete the post so 
instead of PUT we write DELETE and press send but if the person is of user role and doesnt have access or authorization to delete than in that request instead of user we rewrite it as admin to 
check if it really works.

# API Security Excessive Data Exposure 

now if i make a post or make a comment a request is generated to make it happen now if uisng 
burp suite attacker can see all my personal info than API Security Excessive Data Exposure  
happens.

to do this when a request occurs we can right click it and select "Send to Repeater"
and click send button than in response we will see all personal details like id,email,
vehicleID etc.





