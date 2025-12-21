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
than go to payload tab and payload set= 1
payload type = number
**payload settings**
from =1
to = 1000

than start attack

than 1000 otp will go to this mail http://crapi.apisec.ai:8025/






