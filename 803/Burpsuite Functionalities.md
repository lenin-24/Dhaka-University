# Exploring Burpsuite Functionalities

## Firstly after creating a file first need to decide i want to do in default burp browser or local browser like Chrome/firefox
 ### N.B. its better to use firefox rahter than chrome as it is faster
1. IF you want to use local browser than it can be done in 2 ways
     i. Use extension (FoxyProxy- https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-basic/ )
    ii. Go to browser settings & search "Network Settings"
   than click on Manual proxy to put the proxy you need to go to burp proxy settings and copy that address and tick mark the use of proxy for https.    
2. if i manually change proxy multiple times and dont fix it properly than it will create browser problems so its best to use extension.
3. So after installing extension go to FoxyProxy settings , go to proxies tab and than click Add where give title,port and hostname(127.0.0.1) that proxy you got from burp.

## how to see end point request
```
firstly open the default burp browser than go to (ginandjuice.shop)
now if I go to Proxy tab -> Http History than i will be able to see my request
```
## how to block & edit requests 
in Proxy tab -> Intercept i turn on the Intercept button than that webpage will not work.
than in lower tab i will have the option to edit  if i gave name which doesnt exist it will give 404 error

## how to manipulate webpage response

After editing the request message we can do right click on the below proxy edit tab than go to "Do intercept -> Response to this request"

## How to crawl website




